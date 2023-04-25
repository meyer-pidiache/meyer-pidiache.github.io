---
title: Terminal customization
author: meyer
date: 2023-03-14 20:04:00 -0500
categories: [Tutorial]
tags: [linux, terminal, kitty]
---

A user-friendly terminal can make your work easier and more pleasant. In this tutorial, we'll learn how to customize the look and feel of your Linux terminal by using the Kitty terminal emulator and ZSH shell with the Powerlevel10k theme. We'll also install some plugins and tools to improve productivity.

#### Fonts

A user-friendly terminal can make your work easier and more pleasant. In this tutorial, we'll learn how to customize the look and feel of your Linux terminal by using the Kitty terminal emulator and ZSH shell with the Powerlevel10k theme. We'll also install some plugins and tools to improve productivity.

1. Download the HackNerdFont by clicking on this [link](https://github.com/ryanoasis/nerd-fonts/releases/download/v2.3.3/Hack.zip).

2. Unzip the downloaded file to your system fonts folder by running the following commands in the terminal:

    ```shell
    sudo mkdir /usr/share/fonts/HackNerdFont
    unzip ~/Downloads/Hack.zip -d /usr/share/fonts/HackNerdFont
    ```

### Kitty

Kitty is a modern, fast, and feature-rich terminal emulator that supports many advanced features, such as GPU rendering, ligatures, and true-color. You can download and install it from the [official website](https://sw.kovidgoyal.net/kitty/binary/){:target="\_blank"} or with your package manager.

1. Update your package manager:

    ```shell
    sudo apt update
    ```

2. Install Kitty with the following command:

    ```shell
    sudo apt install kitty -y
    ```

3. Create the Kitty configuration file:

    ```shell
    touch ~/.config/kitty/kitty.conf
    ```

4. Open the configuration file with your favorite text editor and add the following settings:

    ```conf
    # Disable bell
    enable_audio_bell no

    # Font
    font_family HackNerdFont
    font_size 12
    url_color #61afef

    # Keyboard shortcuts
    ## Move through windows
    map ctrl+left neighboring_window left
    map ctrl+right neighboring_window right
    map ctrl+up neighboring_window up
    map ctrl+down neighboring_window down

    ## Suspend work
    map ctrl+shift+z toggle_layout stack

    ## New window/tab
    map ctrl+shift+enter new_window_with_cwd
    map ctrl+shift+t new_tab_with_cwd

    # Cursor
    cursor_shape beam
    cursor_beam_thickness 1.8

    # Mouse
    mouse_hide_wait 3.0
    detect_urls yes

    # Input
    repaint_delay 10
    input_delay 3
    sync_to_monitor yes

    # Tabs
    tab_bar_style powerline
    inactive_tab_background #e06c75
    active_tab_background #98c379
    inactive_tab_foreground #000000

    # Window
    background_opacity 0.95
    window_padding_width 4

    # Default shell
    shell zsh

    ```
    {: file='~/.config/kitty/kitty.conf'}

5. Save and close the configuration file.

### ZSH

Zsh is a powerful shell that offers many advanced features and customization options. In this tutorial, we'll use Zsh as our default shell. Install Zsh with the following command:

```shell
sudo apt install zsh -y
```

### PowerLevel10K

[Powerlevel10k](https://github.com/romkatv/powerlevel10k#installation){:target="\_blank"} is a highly customizable Zsh theme that provides a beautiful and informative prompt. We'll install it to enhance the look and feel of our terminal.

![PowerLevel10K](https://raw.githubusercontent.com/romkatv/powerlevel10k-media/master/prompt-styles-high-contrast.png){: width="972" height="589" }
_PowerLevel10K makes zsh better_

```shell
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/.powerlevel10k
echo 'source ~/.powerlevel10k/powerlevel10k.zsh-theme' >> ~/.zshrc
```

> This will clone the repo to `~/.powerlevel10k`
> {: .prompt-info }

### Plugins

For this purpose we will use `locate` to find our plugins path installation

```shell
sudo apt install locate -y
```

Let's proceed with the plugins

- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md){:target="\_blank"}.

```shell
sudo apt install zsh-syntax-highlighting -y
echo "source $(locate zsh-syntax-highlighting.zsh)" > ~.zshrc
```

- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md){:target="\_blank"}.

```shell
sudo apt install zsh-autosuggestions -y
echo "source $(locate zsh-autosuggestions.zsh)" > ~.zshrc
```

- [sudo.plugin](https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/sudo/sudo.plugin.zsh){:target="\_blank"}.

```shell
mkdir -p /usr/share/zsh/plugins/zsh-sudo/
wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/plugins/sudo/sudo.plugin.zsh -P /usr/share/zsh/plugins/zsh-sudo/
echo "source /usr/share/zsh/plugins/zsh-sudo/sudo.plugin.zsh" > ~.zshrc
```

### Batcat

### FZF

### NVIM

#### NVCHAD

### Alias
