---
title: Instalación de Codeium en Nvim con NvChad
author: meyer
date: 2024-02-28 11:01:00 -0500
categories: [Tutorial, Linux]
tags: [linux, terminal, nvim, nvchad, personalización, ia]
---

Una mejor alternativa a Github Copilot es [Codeium](https://github.com/Exafunction/codeium.vim), el cual lo podemos integrar con múltiples entornos gráficos de desarrollo. En este artículo trataré la instalación de Codeium como extensión en Nvim configurado con NvChad. A continuación el paso a paso.

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