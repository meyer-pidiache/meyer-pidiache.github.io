---
title: Recuperar espacio en un VPS de Oracle Cloud: logrotate y reclaim de /var/oled
authors: [meyer]
date: 2026-08-01 18:45:00 -0500
categories: [Tutorial, Linux]
tags: [oracle cloud, linux, lvm, logrotate, kdump, vps]
comments: true
---

> Este artículo ha sido co-creado con inteligencia artificial y probado de punta a punta en un VPS de Oracle Cloud Infrastructure, shape `VM.Standard.A1.Flex`, con Oracle Linux 9 (aarch64).
{: .prompt-info }

Cuando creas una instancia en Oracle Cloud con Oracle Linux, el disco de arranque no llega con un solo sistema de archivos grande. Llega con un esquema LVM donde parte del espacio está **reservado** por Oracle para diagnósticos en `/var/oled`, y a menudo ese espacio se desperdicia mientras `/` se queda sin disco. A eso se le suma que los logs de `syslog` en estas imágenes vienen creciendo sin límite. Este artículo resuelve ambos problemas: configura `logrotate` y devuelve ~14 GB de `/var/oled` al sistema de archivos raíz.

## Contexto: ¿por qué `/` se llena?

El disco de arranque (46.6 GB en el Free Tier) se particiona automáticamente así [^1]:

```
sda (46.6G) → ocivolume (VG, 44.5G)
              ├─ ocivolume-root = 29.5G  → /
              └─ ocivolume-oled = 15G    → /var/oled
```

El LV `oled` está montado en `/var/oled` y Oracle lo destina a datos de diagnóstico del soporte:

- **kdump**: dumps de crash del kernel, configurados en `/etc/kdump.conf` (`path /var/oled/crash`) [^2].
- **PCP** (_Performance Co-Pilot_): los servicios `pmcd`, `pmlogger` y `pmlogger_farm` escriben métricas de rendimiento en `/var/oled/pcp`, según `/etc/pcp.conf` [^3].

En la práctica, un VPS normal usa unos **300 MB** de ese volumen: los ~14 GB restantes quedan reservados pero vacíos, mientras `/` se queda en 98%. La intención de Oracle es garantizar espacio para datos de diagnóstico aunque root se llene [^4], pero para la mayoría de los usuarios es espacio perdido.

> No es un disco aparte: está tallado del **mismo** volumen de arranque. Por eso tu "50 GB" reales se ven como 30 GB en `/`.
{: .prompt-info }

## Riesgos y consideraciones antes de empezar

Esta guía reduce `/var/oled` de 15 GB a 1 GB. Antes de ejecutarla, ten en cuenta lo siguiente:

- **La operación es irreversible para oled.** XFS no se puede achicar en caliente: una vez que el espacio se le da a root, no se puede volver a agrandar `/var/oled` sin reformatear root [^5].
- **El volume group debe tener espacio libre.** En estas imágenes el VG `ocivolume` suele venir al 100% (`VFree 0`), por lo que *no* es posible simplemente "agrandar" oled de 1 GB a 2 GB: habría que quitarle espacio a root, y XFS no lo permite. Si necesitas más espacio para oled, la vía es ampliar el boot volume desde la consola de OCI y usar `oci-growfs` [^6][^7].
- **kdump necesita espacio.** Un vmcore de un sistema de 10 GB de RAM puede acercarse a 1-2 GB incluso comprimido con `makedumpfile -d 31`. Por eso esta guía apunta kdump a `/var/crash` (root), donde hay decenas de GB libres [^2].
- **SELinux está en `Enforcing`** en Oracle Linux: hay que restaurar los contextos con `restorecon`, o los daemons PCP arrancan con contexto roto.
- **Los daemons PCP deben estar detenidos** antes de desmontar el volumen, o `umount` falla por archivos abiertos.
- **No se reinicia en ningún momento** de la guía, y el cambio en `/etc/fstab` se verifica antes de cualquier reboot.

## Paso 1: Configurar logrotate para syslog

En estas imágenes, `logrotate` está instalado y su timer (`logrotate.timer`) corre a diario, pero **no existe configuración para los logs de syslog** (`messages`, `secure`, `cron`). Por eso crecen sin límite: un `messages` de 1.6 GB es normal.

Verifica que el timer esté activo:

```shell
which logrotate && systemctl is-enabled logrotate.timer
```

Crea la configuración (necesita `sudo`, por eso se usa `tee`):

```shell
sudo tee /etc/logrotate.d/syslog > /dev/null <<'EOF'
/var/log/cron
/var/log/maillog
/var/log/messages
/var/log/secure
/var/log/spooler
{
    missingok
    sharedscripts
    weekly
    rotate 4
    compress
    delaycompress
    create 0600 root root
    postrotate
        /bin/kill -HUP `cat /run/rsyslogd.pid 2>/dev/null` 2>/dev/null || true
    endscript
}
EOF
```

El `postrotate` envía `SIGHUP` a `rsyslogd` para que reabra los archivos tras rotarlos. Verifica que la configuración parsee bien:

```shell
sudo logrotate -d /etc/logrotate.d/syslog 2>&1 | grep -E "rotating pattern|error"
```

Para probar el flujo completo de inmediato:

```shell
sudo logrotate -f /etc/logrotate.d/syslog
ls -la /var/log/messages* /var/log/secure*
```

Deberías ver `messages.1`, `secure.1`, etc., y a `rsyslogd` escribiendo en los archivos nuevos.

## Paso 2: Reclamar el espacio de `/var/oled` (15 GB → 1 GB)

**Respaldo de `/etc/fstab`:**

```shell
sudo cp /etc/fstab /etc/fstab.bak
```

### 2.1 Detener los servicios PCP

```shell
sudo systemctl stop pmlogger_farm pmlogger pmie pmcd
sudo fuser -mv /var/oled
```

El segundo comando debe mostrar **solo** `kernel mount`; si aparecen procesos (`pmcd`, `pmlogger`...), espera o detén los servicios restantes.

### 2.2 Desmontar, quitar de fstab y eliminar el LV

```shell
sudo umount /var/oled
sudo sed -i '/ocivolume-oled/d' /etc/fstab
sudo lvremove -y ocivolume/oled
```

> El `-y` es obligatorio: el LV viejo deja una firma XFS residual y LVM pregunta antes de borrarla.
{: .prompt-tip }

### 2.3 Recrear oled de 1 GB, formatear y montar

```shell
sudo lvcreate -y -L 1G -n oled ocivolume
sudo mkfs.xfs -f /dev/mapper/ocivolume-oled
sudo mount /dev/mapper/ocivolume-oled /var/oled
```

### 2.4 Extender root y verificar fstab

```shell
sudo lvextend -l +100%FREE /dev/ocivolume/root
sudo xfs_growfs /
echo '/dev/mapper/ocivolume-oled /var/oled               xfs     defaults        0 0' | sudo tee -a /etc/fstab
sudo findmnt --verify
df -h / /var/oled
```

`findmnt --verify` debe reportar `0 errors`. El warning sobre `/.swapfile` es normal en estas imágenes y es inofensivo.

### 2.5 Recrear la estructura de directorios + SELinux

Los daemons PCP **no arrancan** si faltan sus directorios de log, así que créalos y restaura los contextos de SELinux:

```shell
sudo mkdir -p /var/oled/pcp/{pmcd,pmie,pmlogger,sa}
sudo chown -R pcp:pcp /var/oled/pcp
sudo restorecon -Rv /var/oled
```

### 2.6 Arrancar PCP y verificar

```shell
sudo systemctl start pmcd pmie pmlogger pmlogger_farm
sudo systemctl is-active pmcd pmie pmlogger pmlogger_farm
sudo find /var/oled -type f | head -5
```

Deberías ver `pmcd.log`, `root.log`, etc., recién creados.

## Paso 3: Redirigir kdump a `/var/crash`

Con oled de 1 GB, el vmcore podría no caber (en un sistema de 10 GB de RAM puede acercarse a 1-2 GB). Además, si no hay espacio al momento del crash, **no queda ningún registro del dump** — kdump solo actúa en ese instante y la escritura falla silenciosamente [^2][^8]. Se apunta a `/var/crash`, que vive en root:

```shell
sudo sed -i 's#^path /var/oled/crash#path /var/crash#' /etc/kdump.conf
grep "^path" /etc/kdump.conf
sudo systemctl restart kdump
```

> `systemctl restart kdump` regenera el initramfs del kernel de captura; es obligatorio tras editar `kdump.conf` [^2].
{: .prompt-warning }

Verifica que el kernel de captura quedó cargado:

```shell
cat /sys/kernel/kexec_crash_loaded    # debe imprimir 1
```

## Verificación final

```shell
df -h / /var/oled
sudo vgs && sudo lvs
systemctl is-enabled logrotate.timer
sudo systemctl is-active pmcd pmlogger pmlogger_farm kdump
```

Resultado esperado:

| | Antes | Después |
| :-- | :-- | :-- |
| `/` | 29.5 GB | ~43.5 GB |
| `/var/oled` | 15 GB (2% usado) | 960 MB (5% usado) |
| Logs de syslog | crecen sin límite | rotación semanal + compresión |

## Limitaciones

- **1 GB puede ser poco para un vmcore** en equipos con mucha RAM; por eso kdump apunta a `/var/crash`.
- **No es posible agrandar oled más adelante** sin reformatear root o ampliar el boot volume en la consola de OCI (método oficial: `oci-growfs`) [^6][^7].
- Los datos antiguos de PCP en `/var/oled` se pierden (solo eran métricas de rendimiento, sin valor crítico).
- La guía asume el esquema LVM por defecto (`ocivolume`, LV `oled` de 15 GB). Verifica con `vgs && lvs` que tu instancia lo usa antes de ejecutar.

***

## Referencias

[^1]: [Oracle Community: OCI — What is the ocivolume-oled Volume Mounted on /var/oled](https://community.oracle.com/customerconnect/discussion/656453/oci-what-is-the-ocivolume-oled-volume-mounted-on-var-oled-filesystem){:target="\_blank"}
[^2]: [Oracle Linux 9 — Configuring Kdump](https://docs.oracle.com/en/operating-systems/oracle-linux/9/boot/monitoring-ConfiguringKdump.html){:target="\_blank"}
[^3]: [Oracle Linux 9 — About Performance Co-Pilot](https://docs.oracle.com/en/operating-systems/oracle-linux/9/pcp/about_pcp.html){:target="\_blank"}
[^4]: [Reddit — Reclaiming 10GB /var/oled](https://www.reddit.com/r/oraclecloud/comments/ywwp41/reclaiming_10gb_varoled/){:target="\_blank"}
[^5]: [Oracle Linux — Shrink a LVM root file system](https://support.oracle.com/knowledgefs?docId=1591334){:target="\_blank"}
[^6]: [Oracle Cloud — Extending the Partition for a Boot Volume](https://docs.oracle.com/en-us/iaas/Content/Block/Tasks/extendingbootpartition.htm){:target="\_blank"}
[^7]: [Oracle Cloud — OCI Utilities Reference (oci-growfs)](https://docs.oracle.com/en-us/iaas/oracle-linux/oci/oci-utilities-reference.htm){:target="\_blank"}
[^8]: [The Linux Kernel — Documentation for Kdump (makedumpfile)](https://www.kernel.org/doc/html/latest/admin-guide/kdump/kdump.html){:target="\_blank"}
