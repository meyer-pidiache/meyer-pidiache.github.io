---
title: Personalización de la Terminal en Linux
author: meyer
date: 2023-04-08 20:04:00 -0500
categories: [Tutorial, Linux]
tags: [linux, terminal, kitty, debian, parrrot, arch, manjaro, personalización, zsh, debian]
pin: true
image:
  path: /assets/img/kitty.png
  width: 1920
  height: 1080
  alt: Kitty terminal
---

En este artículo veremos cómo personalizar una terminal en linux, específicamente en aquellas distribuciones basadas en Debian como Parrot Security. y en Arch como Manjaro. Sin tanto preámbulo, empecemos.
> Cualquier error que presente, lo puede dejar en la sección de comentarios.
{: .prompt-warning }

## Tipografía como requisito

### Descarga

Para poder tener una compatibilidad completa con Powerlevel10K necesitamos un tipo de letra monoespaciado, en este caso usaremos la última versión de Hack Nerd Font.

```shell
# Descargamos desde el repositorio oficial
curl -LO https://github.com/ryanoasis/nerd-fonts/releases/latest/download/Hack.zip
# Desempaquetamos los .ttf
mkdir hack-nerd-font
unzip Hack.zip -d hack-nerd-font
rm Hack.zip
```

### Instalación

Ahora que tenemos descargada la fuente, procedemos a instalarla. Una forma sencilla es dar doble click sobre cada archivo y luego seleccionar instalar, pero si queremos complicarnos la vida, podemos hacer lo siguiente.

Usamos un directorio en `/usr/share/fonts/` que almacene las _TrueType Font_ (TTF), este directorio varía de acuerdo al sistema operativo.

```shell
# Debian
sudo mv hack-nerd-font/ /usr/share/fonts/truetype/
# Arch
sudo mv hack-nerd-font/ /usr/share/fonts/TTF/
```

Ahora necesitamos actualizar con `fc-cache` nuestra caché de fuentes, le damos los argumentos de `-f` y `-v` para forzar la actualización y ver más a detalle la ejecución. Finalmente con `fc-list` nos aseguramos de que se ha instalado correctamente nuestra fuente.

```shell
sudo fc-cache -f -v
# Filtramos la lista por "hack"
fc-list | grep "hack"
```

## _Zsh_ como nuestra nueva _shell_

### Instalación

Para poder personalizar al máximo nuestra terminal, dejaremos de usar nuestra _bash_ tradicional para pasarnos a _zsh_, por consiguiente necesitaremos instalarla si no la tenemos.

```shell
# Verificamos si ya la tenemos instalada
zsh --version
# Si no la tenemos, procedemos con la instalación
## Debian
sudo apt update
sudo apt install zsh
## Arch
sudo pacman -Sy
sudo pacman -S zsh
```

### Configuración

Para tener un historial de nuestros comandos ejecutados, crearemos estos archivos de configuración para nuestra nueva _shell_.
```shell
# Si ya existe ~/.zshrc, le hacemos una copia
mv ~/.zshrc ~/.zshrc.old
# Creamos los archivos
touch ~/.zshrc ~/.zsh_history
```

Ahora agregaremos las configuraciones que deseamos al archivo de configuración de la _zsh_, mediante algún editor de texto como `nano` o `vi`.

```conf
HISTSIZE=1000
SAVEHIST=1000
HISTFILE=~/.zsh_history
```
{: file='~/.zshrc'}

## Kitty como nueva terminal

### Instalación

La terminal que voy a usar es [Kitty](https://sw.kovidgoyal.net/kitty/binary/){:target="\_blank"}, la cual tiene muchas opciones que nos permitirá personalizarla a nuestro gusto. Lastimosamente en Arch, a la fecha de actualización de esta publicación, no funciona correctamente esta terminal, así que tendremos que usar otra que queramos.

```shell
# Debian
sudo apt install kitty
```

> Por el momento no vamos abrir esta nueva terminal.
{: .prompt-info }

### Configuración

Con la terminal instalada, ahora podemos configurar algunos detalles de la misma, para mayor información se puede revisar la [documentación](https://sw.kovidgoyal.net/kitty/conf/){:target="\_blank"}. Lo que primero haremos ahora es crear el archivo de configuración o modificar las preferencias de la terminal que estemos usando en Arch, lo que más interesa es el tipo de fuente que vamos a usar.

```shell
# Creamos el directorio de configuración de kitty si no existe
mkdir ~/.config/kitty
# Ahora el archivo
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

Ahora que tenemos nuestra terminal configurada y la fuente requerida, procedemos a instalar [Powerlevel10k](https://github.com/romkatv/powerlevel10k#installation){:target="\_blank"}.

![PowerLevel10K](https://raw.githubusercontent.com/romkatv/powerlevel10k-media/master/prompt-styles-high-contrast.png)
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
# Debian
sudo apt install zsh-syntax-highlighting
# Arch
sudo pacman -S zsh-syntax-highlighting
```

- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md){:target="\_blank"}. Si usamos comandos muy largos, podemos ahorrar tiempo con esta extensión.
```shell
# Debian
sudo apt install zsh-autosuggestions
# Arch
sudo pacman -S zsh-autosuggestions
```

- [sudo.plugin](https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/sudo/sudo.plugin.zsh){:target="\_blank"}. Si queremos ejecutar un comando con sudo, solo tendremos que presionar doble vez la tecla de escape.
```shell
# La etiqueta -p es para crear el directorio 'plugins' que no existe en Debian
sudo mkdir -p /usr/share/zsh/plugins/zsh-sudo/
# Descargamos el plugin
sudo wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/plugins/sudo/sudo.plugin.zsh -P /usr/share/zsh/plugins/zsh-sudo/
```

### Configuración

Ahora estas extensiones ya están en nuestro sistema, pero tenemos que enlazarlas con nuestra zsh, para ello necesitamos conocer la ubicación donde están alojadas, en el caso de Debian las reubicaremos para una mejor organización.

```bash
sudo mv /usr/share/zsh-syntax-highlighting /usr/share/zsh/plugins/
sudo mv /usr/share/zsh-autosuggestions /usr/share/zsh/plugins/
```

> En Arch por defecto, ya está organizado.
> {: .prompt-info }

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
# Debian
sudo apt install bat
# Arch
sudo pacman -S bat
```

- [FZF](https://github.com/junegunn/fzf#installation){:target="\_blank"}. Nos permite buscar archivos y nuestro historial de una manera más dinámica con CTRL+T y CTRL+R. Para habilitar estos atajos, debemos fijarnos en el archivo de configuración que dejaré más abajo.
```shell
# Debian
sudo apt install fzf
# Arch
sudo pacman -S fzf
```

- [LSD](https://github.com/lsd-rs/lsd). Es la versión mejorada de `ls`.
```shell
# Debian
sudo apt install lsd
# Arch
sudo pacman -S lsd
```

- [NVIM](https://github.com/neovim/neovim){:target="\_blank"}. Es la versión mejorada de `vim`, y para usarla en conjunto con NvChad, necesitamos la versión 0.9.4 o superior. A continuación descargamos la última versión y la instalamos.
```shell
# Debian
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux64.tar.gz
sudo rm -rf /opt/nvim
sudo tar -C /opt -xzf nvim-linux64.tar.gz
rm nvim-linux64.tar.gz
# Arch
sudo pacman -S neovim
```

- [NvChad](https://nvchad.com/docs/quickstart/install){:target="\_blank"}. Nos brinda una configuración para `nvim` que es alucinante. Para instalarle plugins a NvChad necesitaremos tener `npm` instalado.
```shell
# Debian 
sudo apt install npm
# Arch
sudo pacman -S npm
# Descarga e instalación de NvChad
git clone https://github.com/NvChad/NvChad ~/.config/nvim --depth 1 && nvim
```
Al primer mensaje que nos aparece, seleccionamos "y" para la configuración por defecto, luego esperamos y ya podremos instalar lo que queramos a NvChad. 
![Instalación de NvChad](/assets/img/NvChad.png)
_Instalación de NvChad_
> Si no hemos presionado la tecla de escape, se nos mostrará un mensaje exitoso, pero si la hemos presionado, podemos esperar un momento a que se instalen los plugins y salir con `!q`.
{: .prompt-info }
⁪
### Configuración

Ahora que tenemos nuevas herramientas, configurémoslas y creemos algunos alias.

```conf
# [...]
export PATH="$PATH:/opt/nvim-linux64/bin"

# alias cat='batcat' # Descomenta para Debian
# alias cat='bat' # Descomenta para Arch 
alias icat='kitty +kitten icat' # Para mostrar imágenes por consola
alias vim='nvim'
# lsd
alias l='lsd --group-dirs=first'
alias la='lsd -a --group-dirs=first'
alias ll='lsd -lh --group-dirs=first'
alias lla='lsd -lha --group-dirs=first'
alias ls='lsd --group-dirs=first'

# Esta función nos ayudará a previsualizar archivos. 'search' desde la terminal para usarla.
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
## Descomenta para Debian
### source /usr/share/doc/fzf/examples/key-bindings.zsh
### source /usr/share/doc/fzf/examples/completion.zsh
## Descomenta para Arch
### source /usr/share/fzf/key-bindings.zsh
### source /usr/share/fzf/completion.zsh


# [...]
```
{: file='~/.zshrc'}

## Último paso

Finalmente, y cruzando los dedos, cerramos nuestra terminal actual y abrimos nuestra _Kitty_ si estamos en Debian. Si estamos usando otra terminal, tendremos que cambiar la shell por defecto de nuestro usuario y reiniciar el sistema.

```shell
# Solo en Arch
chsh -s /bin/zsh
# Para gestionar la terminal podemos usar Tmux
sudo pacman -S tmux
```

Ya en nuestra terminal personalizada, si queremos cambiar el diseño actual solo debemos ejecutar el siguiente comando.

```shell
p10k configure
```

Pero si queremos un nivel más avanzado de personalización, podemos editar el siguiente archivo de configuración.

```conf
vim ~/.p10k.zsh
```
{: file='~/.p10k.zsh'}

Eso es todo, ya tenemos una terminal con superpoderes... Pero no con IA.

## Pequeño regalo

Si queremos integrar un asistente de código a nuestro _nvim_, podemos instalar un plugin llamado [Codeium](https://github.com/Exafunction/codeium.vim). Para su instalación y configuración, tengo en este sitio web un [artículo](https://meyer-pidiache.github.io/posts/nvchad-with-codeium) sobre ello.
