---
title: Alerta Personalizada de Batería Baja en Linux
author: meyer
date: 2023-03-11 02:24:00 -0500
categories: [Tutorial]
tags: [linux, crontab, personalización]
---

## Introducción

Las alertas de batería baja en algunos entornos de escritorios Linux no son muy llamativas, y el estar en un dispositivo el cual queremos cuidar su batería, estas notificaciones pasan por alto en nuestra atención y solo nos volvemos a percatar cuando ya es demasiado tarde. Es por eso que decidí crear esta solución alternativa, ahora que es más fácil crear código para casos de uso sencillos con IA.

## Requisitos

Para que funcione esta alerta personalizada, asegúrese de:
- Tener `zenity` instalado.
- Verificar que la variable `BATTERY_PATH` en el código coincida con su sistema operativo.

> Esta alerta no considera el escenario de contar con múltiples baterías.
{: .prompt-info }

## Vista previa

Para ver una vista previa de esta alerta personalizada, ejecuta:

```shell
zenity --warning \
       --title="¡Batería Baja\!" \
       --text="Conecta el cargador" \
       2>/dev/null
```

## Desarrollo

Primero, crea el archivo que contendrá el código bash y procede a darle permisos de ejecución.

```shell
sudo touch /usr/local/bin/battery_monitor.sh
sudo chmod +x /usr/local/bin/battery_monitor.sh
```

Luego, copia y pega en este el siguiente contenido.

```shell
#!/bin/bash
#
# Author: Meyer Pidiache (github.com/meyer-pidiache)
#

BATTERY_THRESHOLD=20
BATTERY_PATH="/sys/class/power_supply/BAT0"

check_battery() {
    BATTERY_LEVEL=$(cat "$BATTERY_PATH/capacity")
    STATUS=$(cat "$BATTERY_PATH/status")

    if [ "$STATUS" == "Discharging" ] && [ "$BATTERY_LEVEL" -le "$BATTERY_THRESHOLD" ]; then
        return 0
    fi
    return 1
}

while check_battery; do
    zenity --warning \
           --title="¡Batería Baja\!" \
           --text="Conecta el cargador" \
done
```
{: file='/usr/local/bin/battery_monitor.sh'}

### Crontab

Finalmente, creemos una tarea programada con [Crontab](https://linuxhandbook.com/crontab/). Ejecuta:

```shell
crontab -e
```

Para luego agregar esta línea al final del archivo que se está editando:

```conf
*/1 * * * * /usr/local/bin/battery_monitor.sh
```
{: file='crontab -e'}

### Resumen

La tarea programada con Crontab ejecutará el archivo bash cada minuto, y este verificará el estado de la batería, si es menor o igual a 20% (BATTERY_THRESHOLD), se mostrará la alerta. **Si decide cerrar la alerta, esta se volverá a mostrar** hasta que el estado de batería se diferente a descarga, es decir, cuando haya puesto a cargar su dispositivo.

## Eliminar alerta

Para deshacer lo realizado, elimina la línea agregada con `crontab -e` y el archivo ubicado en `/usr/local/bin/battery_monitor.sh`.
