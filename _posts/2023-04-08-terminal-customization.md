---
title: Personalización de la Terminal en Parrot Security 6.0 
author: meyer
date: 2023-04-08 20:04:00 -0500
categories: [Tutorial]
tags: [linux, terminal, kitty, parrrot, personalización, zsh, debian]
pin: true
---

En este artículo veremos cómo personalizar una terminal en linux, específicamente en aquellas distribuciones basadas en Debian como Parrot Security 6.0. Sin tanto preámbulo, empecemos.

## Tipografía como requisito

### Descarga

Para poder tener una compatibilidad completa con Powerlevel10K necesitamos un tipo de letra monoespaciado, en este caso usaremos el recomendado en [repositorio](https://github.com/romkatv/powerlevel10k#fonts){:target="\_blank"} de esta utilidad. Puede descargar los archivos ttf desde los siguientes enlaces o revisar el repositorio indicado.

   - [MesloLGS NF Regular.ttf](
       https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf)
   - [MesloLGS NF Bold.ttf](
       https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf)
   - [MesloLGS NF Italic.ttf](
       https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf)
   - [MesloLGS NF Bold Italic.ttf](
       https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf)

### Instalación

Ahora que tenemos descargada la fuente, procedemos a instalarla. Una forma sencilla es dar doble click sobre cada archivo y luego seleccionar instalar, pero si queremos complicarnos la vida, podemos hacer lo siguiente.

Creamos un directorio en `/usr/share/fonts/truetype` para almacenar nuestra nueva _TrueType Font_ (TTF) y procedemos a mover los archivos a esta carpeta.

```shell
sudo mkdir /usr/share/fonts/truetype/meslo-nerd-font
sudo mv ~/Downloads/MesloLGS* /usr/share/fonts/truetype/meslo-nerd-font/
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
> {: .prompt-info }

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
font_family MesloLGS NF Regular
font_size 12
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
background_opacity 0.95
window_padding_width 4
hide_window_decorations yes # Quitamos los bordes

# Nuestra shell por defecto
shell zsh
```
{: file='~/.config/kitty/kitty.conf'}

### Instalación de PowerLevel10K

Ahora que tenemos nuestra nueva _shell_ y la fuente requerida, procedemos a instalar [Powerlevel10k](https://github.com/romkatv/powerlevel10k#installation){:target="\_blank"}.

![PowerLevel10K](https://raw.githubusercontent.com/romkatv/powerlevel10k-media/master/prompt-styles-high-contrast.png){: width="972" height="589" }
_Fuente: [github.com/romkatv/powerlevel10k](https://github.com/romkatv/powerlevel10k)_

```shell
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/.powerlevel10k
echo 'source ~/.powerlevel10k/powerlevel10k.zsh-theme' >> ~/.zshrc
```

> La ubicación del repositorio estará en `~/.powerlevel10k`
> {: .prompt-info }

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
[...]
source /usr/share/zsh/plugins/zsh-sudo/sudo.plugin.zsh
source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/share/zsh/plugins//zsh-syntax-highlighting.zsh
[...]
```
{: file='~/.zshrc'}

### Herramientas

- [Batcat](https://github.com/sharkdp/bat#installation){:target="\_blank"}. Is a command-line utility that provides syntax highlighting and other features when viewing files. It can be used instead of the standard `cat` command to improve readability of files in the terminal.
    ```shell
sudo apt install bat -y
mkdir -p ~/.local/bin
ln -s /usr/bin/batcat ~/.local/bin/bat
    ```

- [FZF](https://github.com/junegunn/fzf#installation){:target="\_blank"}. Is a command-line fuzzy finder that can be used to quickly search through files, command history, and other sources. It can also be used to select files and directories for use in other commands.

    ```shell
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
    ```

- LSD (short for "LSDeluxe"). Is a modern replacement for the `ls` command in Unix-like operating systems. It aims to improve upon the traditional `ls` command by providing a more colorful and user-friendly output, as well as additional features such as icon glyphs, file grouping, and support for Unicode file names.
    ```shell
sudo apt install lsd -y
    ```

- [NVIM](https://github.com/neovim/neovim){:target="\_blank"}. Is a modern version of the popular text editor Vim. It includes many improvements and new features, including better performance and a more user-friendly interface.
    ```shell
sudo apt install neovim -y
    ```

- [NvChad](https://nvchad.com/docs/quickstart/install){:target="\_blank"}. Is a configuration framework for NVIM that includes a wide range of plugins and settings to make NVIM even more powerful and customizable. It includes features such as auto-completion, syntax highlighting, and code linting, among others.

    > To use more features you will need to have `npm` installed.
    > {: .prompt-info }

    ```shell
git clone https://github.com/NvChad/NvChad ~/.config/nvim --depth 1 && nvim
    ```

### Configuración de atajos (Alias)

An alias is a way to create a shortcut for a command or a set of commands. If you want to use bat instead of `cat` to display text files and want to use `icat` with kitty as the default image viewer, you can set aliases in your `~/.zshrc` file:

```conf
alias cat='bat'
alias icat='kitty +kitten icat'
alias vim='nvim'
# ls
alias l='lsd --group-dirs=first'
alias la='lsd -a --group-dirs=first'
alias ll='lsd -lh --group-dirs=first'
alias lla='lsd -lha --group-dirs=first'
alias ls='lsd --group-dirs=first'
```
{: file='~/.zshrc'}

This means that every time you type `cat` in the terminal, it will run the bat command instead. Similarly, when you type `icat`, it will open the image with kitty.

## Last step

Finally we can close our current terminal and then open our kitty and start customizing our command line, if you want to change the style later you can run:

```shell
p10k configure
```

If you want to play more in detail, simply edit the .p10k.zsh file.

```conf
vim ~/.p10k.zsh
```
{: file='~/.p10k.zsh'}

That's it! Now we have a customizable zshell.
