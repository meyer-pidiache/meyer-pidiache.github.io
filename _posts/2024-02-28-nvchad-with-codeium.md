---
title: Instalación y personalización de Codeium en Nvim
author: meyer
date: 2024-02-28 11:01:00 -0500
categories: [Tutorial, Linux]
tags: [linux, terminal, nvim, nvchad, personalización, ia]
---

Una alternativa a Github Copilot es [Codeium](https://github.com/Exafunction/codeium.vim), el cual lo podemos integrar con múltiples entornos gráficos de desarrollo. En este artículo trataré la instalación de Codeium como extensión en Nvim configurado con NvChad. A continuación el paso a paso.

## Primero necesitaremos una cuenta

Para posteriormente usar este servicio en _nvim_, necesitaremos tener una cuenta creada en [Codeium](https://codeium.com/account/register). Teniendo esto ya hecho, podemos seguir.
 
## Agregamos esta nueva extensión

Procedemos ahora a agregar este coplemento y algunas configuraciones, modificando el archivo `~/.config/nvim/lua/custom/plugins.lua`.

```conf
# [Otras extensiones]
{
  "Exafunction/codeium.vim",
  lazy = false,
  config = function ()
    -- Podemos personalizar '<C-[tecla]>'.
    vim.keymap.set('i', '<C-g>', function () return vim.fn['codeium#Accept']() end, { expr = true, silent = true })
    vim.keymap.set('i', '<C-,>', function() return vim.fn['codeium#CycleCompletions'](1) end, { expr = true, silent = true })
    vim.keymap.set('i', '<C-.>', function() return vim.fn['codeium#CycleCompletions'](-1) end, { expr = true, silent = true })
    vim.keymap.set('i', '<C-x>', function() return vim.fn['codeium#Clear']() end, { expr = true, silent = true })
    vim.keymap.set('i', '<C-i>', function() return vim.fn['codeium#Chat']() end)
  end
},
```
{: file='~/.config/nvim/lua/custom/plugins.lua'}

Hemos agregado los siguientes atajos:
- `Ctrl + g` aceptar una sugerencia.
- `Ctrl + ,` siguiente sugerencia.
- `Ctrl + .` anterior sugerencia.
- `Ctrl + x` quitar sugerencia.
- `Ctrl + i` habilitar chat con IA.

## Nos autenticamos

Ingresamos a Nvim para que se instale el plugin automáticamente, y cuando termine este proceso podremos autenticarnos presionando la tecla de escape y escribiendo `:Codeium Auth`, luego solo tendremos que seguir las instrucciones dadas y listo, ya tenemos este asistente de código instalado.

## Video de referencia

Si queremos ver más información de todo el proceso de instalación, podemos ver el siguiente video el cual nos dará una idea de lo realizado, cabe agregar que hay diferencias notables a lo que hemos hecho.

{% include embed/youtube.html id='L6SJyDMTt4Y' %}

---

## Actualización para LazyVim

Si utilizas [LazyVim](https://www.lazyvim.org/) en lugar de NvChad, el proceso es ligeramente diferente y más estructurado. A continuación se detalla el método recomendado para integrar Codeium.

### 1. Añadir el Plugin y Desactivar Keymaps por Defecto

En LazyVim, cada plugin puede tener su propio archivo de configuración en `lua/plugins/`. Para mantener todo organizado, crea el archivo `lua/plugins/codeium.lua` y desactiva los atajos por defecto.

```lua
-- lua/plugins/codeium.lua
return {
  "Exafunction/codeium.vim",
  event = "BufEnter",
  config = function()
    -- Desactiva los keymaps por defecto para definir los nuestros.
    vim.g.codeium_disable_bindings = 1
  end,
}
```
{: file='~/.config/nvim/lua/plugins/codeium.lua'}

### 2. Configurar Keymaps Personalizados

LazyVim centraliza los atajos de teclado en `lua/config/keymaps.lua`. Añade las siguientes líneas a ese archivo para configurar tus atajos personalizados para Codeium:

```lua
-- lua/config/keymaps.lua

-- (Añadir al final del archivo)

-- Keymaps para Codeium
vim.keymap.set("i", "<C-g>", function() return vim.fn["codeium#Accept"]() end, { expr = true, silent = true, desc = "Codeium: Aceptar" })
vim.keymap.set("i", "<C-,>", function() return vim.fn["codeium#CycleCompletions"](1) end, { expr = true, silent = true, desc = "Codeium: Siguiente" })
vim.keymap.set("i", "<C-.>", function() return vim.fn["codeium#CycleCompletions"](-1) end, { expr = true, silent = true, desc = "Codeium: Anterior" })
vim.keymap.set("i", "<C-x>", function() return vim.fn["codeium#Clear"]() end, { expr = true, silent = true, desc = "Codeium: Limpiar" })
vim.keymap.set("n", "<C-i>", function() vim.fn["codeium#Chat"]() end, { silent = true, desc = "Codeium: Chat" })
```
{: file='~/.config/nvim/lua/config/keymaps.lua'}

### 3. Autenticación

Al igual que con NvChad, después de reiniciar Nvim, ejecuta `:Codeium Auth` para autenticar tu cuenta y empezar a usar el asistente.
