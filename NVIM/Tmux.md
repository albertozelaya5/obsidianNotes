---
tags: [tmux, terminal, atajos]
tema: Tmux - paneles y sesiones en la terminal
---
Tmux divide la terminal en **paneles** (nvim a un lado, `pnpm dev` al otro) y guarda todo en una **sesión** que sigue viva aunque cierres la terminal.

> [!important] El prefijo `Ctrl + a`
> Casi todo empieza con **`Ctrl + a`**: lo presionas, **lo sueltas** y luego la tecla.
> Ejemplo: `Ctrl + a`, soltar, `-` = dividir abajo.
> **Excepción:** moverse entre paneles es `Ctrl + h/j/k/l` **sin prefijo**.

## 1. Entrar y salir (en la terminal)

| Comando                     | Qué hace                                        |
| --------------------------- | ----------------------------------------------- |
| `tmux new -A -s pyme`       | Entra a la sesión "pyme" (la crea si no existe) |
| `Ctrl + a` → `d`            | **Salir** dejándola viva de fondo               |
| `tmux ls`                   | Ver sesiones abiertas                           |
| `tmux kill-session -t pyme` | Cerrar la sesión para siempre                   |

## 2. Paneles (lo del día a día)

| Atajo                                    | Qué hace                                                                     |
| ---------------------------------------- | ---------------------------------------------------------------------------- |
| `Ctrl + a` → `\`                         | Nuevo panel a la **derecha**                                                 |
| `Ctrl + a` → `-`                         | Nuevo panel **abajo**                                                        |
| `Ctrl + h / j / k / l`                   | Moverte ← ↓ ↑ → (funciona igual entre nvim y tmux)                           |
| `Ctrl + a` → `z`                         | **Maximizar / restaurar** el panel                                           |
| `Ctrl + a` → `x`                         | Cerrar el panel (o escribe `exit`)                                           |
| `Ctrl + a` → `H / J / K / L`             | Mover el borde del panel (repite la letra sin volver a presionar `Ctrl + a`) |
| `Ctrl + a` → `Ctrl + l`                  | Limpiar la pantalla                                                          |
| `Ctrl+a `→ `H / J / K / L ` (mayúsculas) | Agrandar o achicar de 5 en 5                                                 |

> [!tip] Con el mouse
> **Clic** cambia de panel, **arrastrar el borde** cambia el tamaño, **rueda** hace scroll (`q` para salir del scroll).
> Para copiar texto: mantén **`Option`** mientras seleccionas y luego `Cmd + c`.

## 3. Ventanas (pestañas)

| Atajo | Qué hace |
|---|---|
| `Ctrl + a` → `c` | Nueva ventana |
| `Ctrl + a` → `1`, `2`, … | Ir a la ventana 1, 2… |

## 4. Flujo diario (proyecto PYME)

```sh
cd ~/Documents/banhcafe-projects/BANHCAFE-Pyme
tmux new -A -s pyme
nvim .
```

1. `Ctrl + a` → `\` y en el panel nuevo: `pnpm dev`
2. `Ctrl + a` → `-` para otro panel abajo: git, `pnpm lint`…
3. `Ctrl + h / l` para ir y venir entre nvim y las terminales
4. Al terminar: `Ctrl + a` → `d`. Mañana `tmux new -A -s pyme` y todo sigue igual.

```
┌──────────────────────┬──────────────┐
│                      │  pnpm dev    │
│       nvim .         ├──────────────┤
│                      │  git / lint  │
└──────────────────────┴──────────────┘
```

## 5. Si algo no funciona

- **`Ctrl + a` no hace nada:** no estás dentro de tmux (`echo $TMUX` sale vacío). Entra con `tmux new -A -s pyme`.
- **El prefijo son dos pasos:** `Ctrl + a`, **soltar**, y luego la tecla.
- **Editaste `~/.tmux.conf`:** `Ctrl + a` → `r` para recargarla.

Relacionado: [[Atajos Lazyvim]] · [[Terminal]] · [[Lazygit]]
