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

## 6. En Windows (WezTerm)

En Windows no hay tmux: se usa **WezTerm** configurado igual. Mismo prefijo **`Ctrl + a`** y mismos atajos de las secciones 2 y 3.

> [!info] Dónde está la configuración
> - Atajos, sesiones y guardado: `~/.wezterm.lua`
> - Comandos `tmux` en PowerShell: `~/.wezterm-tmux.ps1`, cargado desde el perfil (`notepad $PROFILE` → línea `. "$HOME\.wezterm-tmux.ps1"`)
> - En nvim: **smart-splits.nvim** en lugar de vim-tmux-navigator, así `Ctrl + h / j / k / l` cruza entre nvim y los paneles de WezTerm

| En tmux (Mac)                       | En WezTerm (Windows)                                                        |
| ----------------------------------- | --------------------------------------------------------------------------- |
| `tmux new -A -s pyme`               | Igual. Fuera de WezTerm, abre WezTerm directo en esa sesión                 |
| `tmux ls` · `tmux a -t pyme`        | Igual                                                                       |
| `tmux kill-session -t pyme`         | Igual                                                                       |
| `Ctrl + a` → `d`                    | Igual (o `tmux detach`)                                                     |
| Ventanas `Ctrl + a` → `c`, `1`, `2` | Igual, se ven como **pestañas** abajo                                       |
| Copiar con `Option` + `Cmd + c`     | Seleccionar con el mouse y `Ctrl + Shift + c`, o `Ctrl + a` → `[` (`v` selecciona, `y` copia, `q` sale) |
| `Ctrl + a` en la shell (inicio de línea) | `Ctrl + a` **dos veces** (una sola es el prefijo)                      |

**Extras de WezTerm:** `Ctrl + a` → `q` numera los paneles para saltar a uno · `Ctrl + a` → `N` nueva sesión con nombre · `Ctrl + a` → `s` cambiar de sesión · `tmux rename-session -t pyme nuevo`.

### Revivir después de apagar la compu

Funciona como tmux-resurrect + tmux-continuum, sin instalar nada más:

- Cada **5 minutos** (y al salir con `Ctrl + a` → `d`) se guardan todas las sesiones: paneles, cómo están divididos, la carpeta de cada uno y si tenía nvim abierto.
- Al prender la compu y **abrir WezTerm**, las sesiones vuelven solas en unos segundos.
- A mano: `Ctrl + a` → `Ctrl + s` guarda · `Ctrl + a` → `Ctrl + r` restaura (también `tmux save` / `tmux restore`).

> [!warning] Lo que no vuelve
> Los programas que estaban corriendo, salvo nvim: `pnpm dev` hay que lanzarlo de nuevo. Lo hecho después del último guardado (máximo 5 min) se pierde.

### Si algo no funciona en Windows

- **`tmux` no se reconoce:** falta la línea en el perfil de PowerShell (ver arriba) y abrir una terminal nueva.
- **`Ctrl + h / j / k / l` no sale de nvim:** `:SmartSplitsLog` dentro de nvim muestra el error.
- **Editaste `~/.wezterm.lua`:** se recarga solo al guardar; si no, `Ctrl + a` → `r`.

Relacionado: [[Atajos Lazyvim]] · [[Terminal]] · [[Lazygit]]
