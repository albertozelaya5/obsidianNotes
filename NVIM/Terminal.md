---
tags: [neovim, terminal, atajos]
tema: Terminales en Neovim
---

# Terminales en Neovim

## 1. Abrir una terminal

| Comando               | Qué hace                                    |
| --------------------- | ------------------------------------------- |
| `:terminal` o `:term` | Abre la terminal en la ventana actual       |
| `:split \| terminal`  | Abre la terminal en una división horizontal |
| `:vsplit \| terminal` | Abre la terminal en una división vertical   |
| `:tabnew \| terminal` | Abre la terminal en una pestaña nueva       |

> [!tip] Empezar a escribir
> Al abrirse, la terminal está en **modo normal**. Presiona `i` para entrar en **modo terminal** y escribir comandos.

> [!important] Salir del modo terminal
> `Ctrl+\` seguido de `Ctrl+n` → vuelves al modo normal para moverte entre ventanas.

## 2. Cambiar el foco entre ventanas y terminales

| Atajo | Destino |
|---|---|
| `Ctrl+w h` | Ventana de la izquierda |
| `Ctrl+w j` | Ventana de abajo |
| `Ctrl+w k` | Ventana de arriba |
| `Ctrl+w l` | Ventana de la derecha |
| `Ctrl+w w` | Siguiente ventana (en ciclo) |

> [!note] Recuerda
> Si estás escribiendo dentro de la terminal, primero sal con `Ctrl+\` `Ctrl+n` y después usa `Ctrl+w`.

### Si usas pestañas

| Atajo | Qué hace |
|---|---|
| `gt` | Siguiente pestaña |
| `gT` | Pestaña anterior |

## 3. Atajo cómodo (opcional)

Para saltar desde la terminal a otra ventana sin salir primero del modo terminal, agrega esto a `init.lua`:

```lua
vim.keymap.set('t', '<C-h>', '<C-\\><C-n><C-w>h')
vim.keymap.set('t', '<C-j>', '<C-\\><C-n><C-w>j')
vim.keymap.set('t', '<C-k>', '<C-\\><C-n><C-w>k')
vim.keymap.set('t', '<C-l>', '<C-\\><C-n><C-w>l')
```

Ahora `Ctrl+h/j/k/l` te mueve directo entre ventanas desde la terminal.

## 4. Extra: suspender Neovim

- `Ctrl+z` → suspende Neovim y vuelves a tu terminal original.
- `fg` → regresas a Neovim.

## Resumen rápido

- Abrir terminal: `:term`
- Escribir en ella: `i`
- Salir del modo terminal: `Ctrl+\` `Ctrl+n`
- Moverte entre ventanas: `Ctrl+w` + `h/j/k/l`