---
title: Personalización de la Terminal en Parrot Security 6.0
author: meyer
date: 2023-04-08 20:04:00 -0500
categories: [Tutorial]
tags: [linux, terminal, kitty, parrrot, personalización, zsh, debian]
pin: true
image:
  path: /assets/img/kitty.png
  width: 1920
  height: 1080
  alt: Kitty terminal
---

En este artículo veremos cómo personalizar una terminal en linux, específicamente en aquellas distribuciones basadas en Debian como Parrot Security 6.0. Sin tanto preámbulo, empecemos.
> Cualquier error que presente, lo puede dejar en la sección de comentarios.
{: .prompt-warning }

## Tipografía como requisito

### Descarga

Para poder tener una compatibilidad completa con Powerlevel10K necesitamos un tipo de letra monoespaciado, en este caso usaremos la última versión de Hack Nerd Font.

```
curl -LO https://github.com/ryanoasis/nerd-fonts/releases/latest/download/Hack.zip
mkdir hack-nerd-font
unzip Hack.zip -d hack-nerd-font
rm Hack.zip
```

### Instalación

Ahora que tenemos descargada la fuente, procedemos a instalarla. Una forma sencilla es dar doble click sobre cada archivo y luego seleccionar instalar, pero si queremos complicarnos la vida, podemos hacer lo siguiente.

Creamos un directorio en `/usr/share/fonts/truetype` para almacenar nuestra nueva _TrueType Font_ (TTF) y procedemos a mover los archivos a esta carpeta.

```shell
sudo mv hack-nerd-font/ /usr/share/fonts/truetype/
```

Ahora necesitamos actualizar con `fc-cache` nuestra caché de fuentes, le damos los argumentos de `-f` y `-v` para forzar la actualización y ver más a detalle la ejecución. Finalmente con `fc-list` nos aseguramos de que se ha instalado correctamente nuestra fuente.

```shell
sudo fc-cache -f -v
fc-list | grep "meslo"
```

## _Zsh_ como nuestra nueva _shell_

### Instalación

Para poder personalizar al máximo nuestra terminal, dejaremos de usar nuestra _bash_ tradicional para pasarnos a _zsh_, por consiguiente necesitaremos instalarla.

```shell
sudo apt update
sudo apt install zsh
```

### Configuración

Para tener un historial de nuestros comandos ejecutados, crearemos estos archivos de configuración para nuestra nueva _shell_.
```shell
touch ~/.zshrc ~/.zsh_history
```

Ahora agregaremos las configuraciones que deseamos a este archivo.

```conf
HISTSIZE=1000
SAVEHIST=1000
HISTFILE=~/.zsh_history
```
{: file='~/.zshrc'}

## Kitty como nueva terminal

### Instalación

La terminal que voy a usar es [Kitty](https://sw.kovidgoyal.net/kitty/binary/){:target="\_blank"}, la cual tiene muchas opciones que nos permitirá personalizarla a nuestro gusto. Para instalarla lo podemos hacer mediante `apt`.

```shell
sudo apt install kitty
```

> Por el momento no vamos abrir esta nueva terminal.
{: .prompt-info }

### Configuración

Con la terminal instalada, ahora podemos configurar algunos detalles de la misma, para mayor información se puede revisar la [documentación](https://sw.kovidgoyal.net/kitty/conf/){:target="\_blank"}. Lo que primero haremos ahora es crear el archivo de configuración.

```shell
touch ~/.config/kitty/kitty.conf
```

Con el archivo ya creado, lo que tendremos que hacer ahora es editarlo con las configuraciones que deseemos, a continuación dejo un ejemplo detallado.

```conf
# Deshabilitamos el sonidito de la campana
enable_audio_bell no

# Establecemos la tipografía que vamos a usar
font_family Hack Nerd Font
font_size 14
url_color #61afef

# Personalizamos los atajos del teclado
## Movimientos entre ventanas (ctr + alguna flechita)
map ctrl+left neighboring_window left
map ctrl+right neighboring_window right
map ctrl+up neighboring_window up
map ctrl+down neighboring_window down

## Suspender trabajo
map ctrl+shift+z toggle_layout stack

## Crear una nueva ventana o pestaña con el directorio actual
map ctrl+shift+enter new_window_with_cwd
map ctrl+shift+t new_tab_with_cwd

# Estilo del cursor
cursor_shape beam
cursor_beam_thickness 1.8

# Comportamiento del ratón
mouse_hide_wait 3.0
detect_urls yes

# Entrada por teclado
repaint_delay 10
input_delay 3
sync_to_monitor yes

# Estilo de las pestañas
tab_bar_style powerline
inactive_tab_background #e06c75
active_tab_background #98c379
inactive_tab_foreground #000000

# Estilo de la terminal
background_opacity 0.85
window_padding_width 4

# Nuestra shell por defecto
shell zsh
```
{: file='~/.config/kitty/kitty.conf'}

## PowerLevel10K

### Instalación

Ahora que tenemos nuestra nueva _shell_ y la fuente requerida, procedemos a instalar [Powerlevel10k](https://github.com/romkatv/powerlevel10k#installation){:target="\_blank"}.

![PowerLevel10K](https://raw.githubusercontent.com/romkatv/powerlevel10k-media/master/prompt-styles-high-contrast.png){: width="972" height="589" }
_Fuente: [github.com/romkatv/powerlevel10k](https://github.com/romkatv/powerlevel10k)_

```shell
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/.powerlevel10k
```

> La ubicación del repositorio estará en `~/.powerlevel10k`
> {: .prompt-info }

### Configuración

Para este caso, ya tengo predefinidas algunas [configuraciones](https://github.com/meyer-pidiache/dotfiles/blob/main/.p10k.zsh){:target="\_blank"} estéticas para nuestra nueva shell, para establecerlas necesitaremos descargar un archivo.

```bash
wget https://raw.githubusercontent.com/meyer-pidiache/dotfiles/main/.p10k.zsh -O ~/.p10k.zsh
```

Ahora debemos modificar el archivo de configuración de nuestra _zsh_.

```conf
# Esta parte debe estar en el inicio del archivo
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi

# [...]

# Enlazamos nuestra Powerlevel10k al final
source ~/.powerlevel10k/powerlevel10k.zsh-theme

[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh
```
{: file='~/.zshrc'}

## Extensiones

### Instalación

Para tener más funcionalidades, podemos instalar las siguientes extensiones (_plugins_) que nos darán un toque especial a nuestra zsh.

- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md){:target="\_blank"}. Los colores le dan vida a una terminal, y esta extensión hará su trabajo.
```shell
sudo apt install zsh-syntax-highlighting
```

- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md){:target="\_blank"}. Si usamos comandos muy largos, podemos ahorrar tiempo con esta extensión.
```shell
sudo apt install zsh-autosuggestions
```

- [sudo.plugin](https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/sudo/sudo.plugin.zsh){:target="\_blank"}. Si queremos ejecutar un comando con sudo, solo tendremos que presionar doble vez la tecla de escape.
```shell
sudo mkdir -p /usr/share/zsh/plugins/zsh-sudo/
sudo wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/plugins/sudo/sudo.plugin.zsh -P /usr/share/zsh/plugins/zsh-sudo/
```

### Configuración

Ahora estas extensiones ya están en nuestro sistema, pero tenemos que enlazarlas con nuestra zsh, para ello necesitamos conocer la ubicación donde están alojadas, en el caso de Parrot las reubicaremos para una mejor organización.

> Debemos asegurarnos de la existencia del directorio `/usr/share/zsh/plugins/` para continuar.
> {: .prompt-warning }

```bash
sudo mv /usr/share/zsh-* /usr/share/zsh/plugins/
```

Con lo anterior hecho, agregaremos las ubicaciones de las extensiones a nuestro arhivo de configuración de la zsh.

```conf
# [...]

source /usr/share/zsh/plugins/zsh-sudo/sudo.plugin.zsh
source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

# [...]
```
{: file='~/.zshrc'}

## Herramientas

Si no nos parece suficiente con las extensiones y queremos más, podemos instalar herramientas que nos serán de gran utilidad.
### Instalación
- [Batcat](https://github.com/sharkdp/bat#installation){:target="\_blank"} 
Este programa nos permite visualizar archivos de manera más amigable, es un reemplazo mejorado de `cat`.
```shell
sudo apt install bat
```

- [FZF](https://github.com/junegunn/fzf#installation){:target="\_blank"}. Nos permite buscar archivos y nuestro historial de una manera más dinámica.
```shell
sudo apt install fzf
```

- [LSD](https://github.com/lsd-rs/lsd). Es la versión mejorada de `ls`.
```shell
sudo apt install lsd
```

- [NVIM](https://github.com/neovim/neovim){:target="\_blank"}. Es la versión mejorada de `vim`, y para usarla en conjunto con NvChad, necesitamos la versión 0.9.4 o superior. A continuación descargamos la última versión y la instalamos.
```shell
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux64.tar.gz
sudo rm -rf /opt/nvim
sudo tar -C /opt -xzf nvim-linux64.tar.gz
rm nvim-linux64.tar.gz
```

- [NvChad](https://nvchad.com/docs/quickstart/install){:target="\_blank"}. Nos brinda una configuración para `nvim` que es alucinante.
```shell
git clone https://github.com/NvChad/NvChad ~/.config/nvim --depth 1 && nvim
```
Al primer mensaje que nos aparece, seleccionamos "y" para la configuración por defecto.
> Para instalar plugins adicionales, asegurémonos de tener `npm` instalado.
> {: .prompt-info }

### Configuración

Ahora que tenemos nuevas herramientas, configurémoslas y creemos algunos alias.

```conf
# [...]
export PATH="$PATH:/opt/nvim-linux64/bin"

alias cat='batcat'
alias icat='kitty +kitten icat' # Para mostrar imágenes por consola
alias vim='nvim'
# lsd
alias l='lsd --group-dirs=first'
alias la='lsd -a --group-dirs=first'
alias ll='lsd -lh --group-dirs=first'
alias lla='lsd -lha --group-dirs=first'
alias ls='lsd --group-dirs=first'

function search(){
	if [ "$1" = "h" ]; then

		fzf -m --reverse --preview-window down:20 --preview '[[ $(file --mime {}) =~ binary ]] &&

 	                echo {} is a binary file ||

	                 (batcat --style=numbers --color=always {} ||

	                  highlight -O ansi -l {} ||

	                  coderay {} ||

	                  rougify {} ||

	                  cat {}) 2> /dev/null | head -500'

	else

	        fzf -m --preview '[[ $(file --mime {}) =~ binary ]] &&

	                         echo {} is a binary file ||

	                         (batcat --style=numbers --color=always {} ||

	                          highlight -O ansi -l {} ||

	                          coderay {} ||

	                          rougify {} ||

	                          cat {}) 2> /dev/null | head -500'

	fi
}

# Atajos
## Avanzar (alt+right)
bindkey "^[[1;3C" forward-word
## Retroceder (alt+left)
bindkey "^[[1;3D" backward-word
## Eliminar bloque adelante (alt+supr)
bindkey "^[[3;3~" delete-word

# FZF
source /usr/share/doc/fzf/examples/key-bindings.zsh
source /usr/share/doc/fzf/examples/completion.zsh

# [...]
```
{: file='~/.zshrc'}

Tenemos una función `search` que nos ayudará a previsualizar archivos.

## Último paso

Finalmente, y cruzando los dedos, cerramos nuestra terminal actual y abrimos nuestra _Kitty_. Si queremos cambiar el diseño actual de la terminal, podemos ejecutar el siguiente comando.

```shell
p10k configure
```

Si queremos un nivel más avanzado de personalización, podemos editar el siguiente archivo de configuración.

```conf
vim ~/.p10k.zsh
```
{: file='~/.p10k.zsh'}

Eso es todo, ya tenemos una terminal con superpoderes.
