---
title: Instalación de Docker en Parrot OS
author: meyer
date: 2023-03-14 20:04:00 -0500
categories: [Tutorial]
tags: [linux, parrot, docker]
---

## Instalación utilizando el repositorio apt

Antes de instalar Docker Engine por primera vez en una nueva máquina, se necesita configurar el repositorio Docker. Después, ya se puede instalar y actualizar Docker fácilmente.

### Configurar el repositorio apt de Docker

```shell
# Actualizar la lista de paquetes
sudo apt-get update

# Instalar certificados y herramientas
sudo apt-get install ca-certificates curl

# Descargar e instalar la clave GPG de Docker
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

### Agregar el repositorio

```shell
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  buster stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

> En el caso de Parrot OS usamos `buster` en lugar de `(. /etc/os-release && echo "$VERSION_CODENAME")`.
{: .prompt-info }

### Instalación

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Listo, ahora podemos usar docker pero con sudo.

```shell
sudo docker run hello-world
```

Para ejecutarlo como usuario normal, necesitamos agregar nuestro usuario al grupo `docker`, y crear este grupo si no se ha creado.

```shell
sudo groupadd docker
sudo usermod -aG docker $USER
```

Para que los cambios se apliquen, reiniciamos sesión o ejecutamos lo siguiente:

```shell
newgrp docker
```

Ahora ya deberíamos poder ejecutar `docker` sin `sudo`, para más información se puede revisar la [documentación](https://docs.docker.com/engine/install/linux-postinstall/). 
