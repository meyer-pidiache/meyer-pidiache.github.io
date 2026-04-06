---
title: Instalación de Docker en Parrot OS
authors: [meyer]
date: 2023-03-14 20:04:00 -0500
categories: [Tutorial]
tags: [linux, parrot os, docker]
comments: true
---

> Este artículo ha sido co-creado con inteligencia artificial.
{: .prompt-info }

Parrot OS es una distribución de Linux basada en Debian que, si bien no cuenta con soporte oficial para Docker, permite la instalación de Docker Engine utilizando el repositorio apt de Docker para Debian [^1]. La clave está en utilizar el _codename_ de la versión base de Debian que Parrot OS emplea, en lugar del _codename_ que Parrot reporta en su propio sistema.

Para identificar qué versión base utiliza tu instalación de Parrot OS, puedes ejecutar `cat /etc/os-release`. La siguiente tabla muestra la relación entre las versiones de Parrot OS y sus bases de Debian asociadas[^2][^3][^4][^5][^6]:

| Versión Parrot OS | Versión Debian base | Codename Debian (`VERSION_CODENAME`) |
| :-- | :-- | :-- |
| **Parrot 3** | Debian Testing | `stretch` (Testing en ese momento) |
| **Parrot 4** | Debian Testing | `buster` (Testing en ese momento) |
| **Parrot 5** | Debian 11 Stable | `bullseye` |
| **Parrot 6** | Debian 12 Stable | `bookworm` |
| **Parrot 7** | Debian 13 Stable | `trixie` |

## Prerrequisitos

### Requisitos del sistema

### Desinstalar versiones anteriores

Este comando elimina los paquetes que podrían entrar en conflicto con la instalación oficial de Docker Engine[^1].

```shell
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-doc podman-docker containerd runc | cut -f1)
```

## Instalación mediante repositorio apt

Antes de instalar Docker Engine por primera vez en una nueva máquina, se debe configurar el repositorio apt de Docker. Posterior a esto, la instalación y actualización de Docker será directa.

### Configurar el repositorio apt de Docker

El primer paso es agregar la clave GPG oficial de Docker:

```shell
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

### Agregar el repositorio

Una vez configurada la clave GPG, se agrega el repositorio a las fuentes de apt. Es crucial especificar manualmente el _codename_ de Debian correspondiente a la versión base de Parrot OS que se esté utilizando:

```shell
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

> En Parrot OS, la variable `VERSION_CODENAME` reportada por el sistema no corresponde a una versión de Debian soportada por Docker. Por esta razón, es necesario sustituir manualmente el _codename_ según la tabla de versiones proporcionada.
{: .prompt-warning }

### Instalación de Docker Engine

Con el repositorio configurado, se procede a instalar los paquetes de Docker:

```shell
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Verificación de la instalación

Una vez completada la instalación, Docker Engine se inicia automáticamente en sistemas basados en Debian. Para verificar que el servicio está corriendo:

```shell
sudo systemctl status docker
```

Para verificar la instalación, se puede ejecutar la imagen de prueba:

```shell
sudo docker run hello-world
```

Este comando descarga una imagen de prueba y la ejecuta en un contenedor. Si la instalación fue exitosa, se mostrará un mensaje de confirmación[^1].

## Configuración post-instalación

Por defecto, ejecutar comandos Docker requiere privilegios de administrador (sudo). Para permitir que usuarios sin privilegios ejecuten Docker, se debe agregar el usuario al grupo `docker`:

```shell
sudo groupadd docker
sudo usermod -aG docker $USER
```

Para aplicar los cambios sin cerrar sesión:

```shell
newgrp docker
```

Tras esto, debería ser posible ejecutar `docker` sin `sudo`[^1][^7].

> Es importante considerar las implicaciones de seguridad de agregar usuarios al grupo `docker`. Los usuarios con acceso al grupo `docker` pueden escalar privilegios de forma efectiva, ya que tienen acceso al socket de Docker con permisos de root. Para entornos donde esto representa un riesgo inaceptable, Docker ofrece la opción de ejecutar el daemon en modo rootless[^7].
{: .prompt-info }

## Desinstalación

Para desinstalar Docker Engine completamente:

```shell
sudo apt purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Para eliminar imágenes, contenedores y volúmenes:

```shell
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

Para remover el repositorio y las claves:

```shell
sudo rm /etc/apt/sources.list.d/docker.sources
sudo rm /etc/apt/keyrings/docker.asc
```

***

## Referencias

[^1]: [https://docs.docker.com/engine/install/debian/](https://docs.docker.com/engine/install/debian/){:target="\_blank"}
[^2]: [https://parrot.sh/blog/2024-01-24-parrot-6.0-release-notes](https://parrot.sh/blog/2024-01-24-parrot-6.0-release-notes){:target="\_blank"}
[^3]: [https://parrot.sh/blog/2022-03-24-parrot-5.0-press-release](https://parrot.sh/blog/2022-03-24/parrot-5.0-press-release){:target="\_blank"}
[^4]: [https://parrot.sh/blog/2018-05-21/parrot-4-0-release-notes/](https://parrot.sh/blog/2018-05-21/parrot-4-0-release-notes/){:target="\_blank"}
[^5]: [https://parrot.sh/blog/2016-10-15-parrot-3-2-released/](https://parrot.sh/blog/2016-10-15/parrot-3-2-released/){:target="\_blank"}
[^6]: [https://cyberpress.org/parrot-7-0-beta-released/](https://cyberpress.org/parrot-7-0-beta-released/){:target="\_blank"}
[^7]: [https://docs.docker.com/engine/install/linux-postinstall/](https://docs.docker.com/engine/install/linux-postinstall/){:target="\_blank"}
