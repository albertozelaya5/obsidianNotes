---
tags: [tmux, terminal, atajos]
tema: Tmux - paneles y sesiones en la terminal
---
Tmux divide la terminal en **paneles** y **ventanas** y guarda todo en **sesiones** que siguen vivas aunque cierres la terminal. Ideal para tener `nvim` a un lado y `pnpm dev` al otro.

> [!info] Instalación (ya hecho)
> `brew install tmux` → config amigable en `~/.tmux.conf` (ver sección 6).

## 1. La idea en 30 segundos

```
Sesión  (un proyecto: "pyme")
 └─ Ventana  (como una pestaña: "código", "logs")
     └─ Panel  (divisiones dentro de la ventana)
```

> [!important] El prefijo
> Casi todo empieza con **`Ctrl+a`**: lo presionas, **lo sueltas** y luego la tecla.
> Ejemplo: `Ctrl+a` → `|` = dividir vertical.
>
> **Excepción:** moverse entre paneles es `Ctrl+h/j/k/l` **sin prefijo** (ver sección 8).

## 2. Sesiones (se escriben en la terminal)

| Comando                   | Qué hace                              |
| ------------------------- | ------------------------------------- |
| `tmux new -A -s pyme`     | Entra a "pyme" (la crea si no existe) |
| `tmux new -s pyme`        | Crea la sesión "pyme"                 |
| `tmux ls`                 | Lista sesiones abiertas               |
| `tmux attach -t pyme`     | Vuelve a la sesión "pyme"             |
| `tmux a`                  | Vuelve a la última sesión             |
| `tmux kill-session -t pyme` | Cierra la sesión                    |

Dentro de tmux:

| Atajo        | Qué hace                                         |
| ------------ | ------------------------------------------------ |
| `Ctrl+a d`   | **Salir** de la sesión (sigue corriendo de fondo) |
| `Ctrl+a s`   | Ver y cambiar entre sesiones                     |

## 3. Paneles (lo que más vas a usar)

| Atajo              | Qué hace                             |
| ------------------ | ------------------------------------ |
| `Ctrl+a \|` o `Ctrl+a \` | Dividir **vertical** (panel a la derecha) |
| `Ctrl+a -`         | Dividir **horizontal** (panel abajo) |
| `Ctrl+h/j/k/l`     | Moverse ← ↓ ↑ → **sin prefijo** (también desde nvim) |
| `Ctrl+a h/j/k/l`   | Moverse ← ↓ ↑ → (versión con prefijo, también funciona) |
| `Ctrl+a Ctrl+l`    | Limpiar la pantalla (antes era `Ctrl+l`) |
| `Ctrl+a H/J/K/L`   | **Redimensionar** de 5 en 5: mueve el borde ← ↓ ↑ → (repite la letra sin el prefijo) |
| `Ctrl+a Shift+flecha` | **Redimensionar** de 1 en 1 (ajuste fino, repetible) |
| `Ctrl+a z`         | **Maximizar / restaurar** el panel   |
| `Ctrl+a x`         | Cerrar panel (o escribe `exit`)      |
| `Ctrl+a espacio`   | Cambiar el acomodo de los paneles    |

> [!tip] Con mouse
> El mouse está activado: **clic** para cambiar de panel, **arrastrar el borde** para redimensionar y **rueda** para hacer scroll.

### Redimensionar paneles

| Paso | Atajo |
| --- | --- |
| **De 5 en 5** | `Ctrl+a`, luego `H` / `J` / `K` / `L` (mayúsculas) |
| **De 1 en 1** | `Ctrl+a`, luego `Shift+←` / `↓` / `↑` / `→` |
| **Porcentaje exacto** | `Ctrl+a :` → `resize-pane -x 30%` (`-y` para el alto) |
| **Con el mouse** | Arrastrar el borde entre paneles |

La tecla se puede **repetir sin volver a presionar `Ctrl+a`** si la presionas antes de ~0.5 s. Ejemplo: `Ctrl+a`, luego `H H H`.

> [!important] La tecla mueve el **borde** hacia esa dirección, no "agranda el panel"
> Por eso en el panel de la derecha `L` lo **achica**: el borde se va hacia la derecha.

| Panel | Para agrandar | Para achicar |
| --- | --- | --- |
| Izquierda (nvim) | `L` / `Shift+→` | `H` / `Shift+←` |
| Derecha | `H` / `Shift+←` | `L` / `Shift+→` |
| Arriba | `J` / `Shift+↓` | `K` / `Shift+↑` |
| Abajo | `K` / `Shift+↑` | `J` / `Shift+↓` |

> [!warning] Atajos de fábrica que fallan en Mac
> `Ctrl+a Ctrl+flecha` y `Ctrl+a Option+flecha` suelen no funcionar: macOS usa `Ctrl+←/→` para cambiar de escritorio y Warp no manda `Option` como `Alt` por defecto. Por eso se usan `H/J/K/L` y `Shift+flecha`.

## 4. Ventanas (pestañas)

| Atajo         | Qué hace                      |
| ------------- | ----------------------------- |
| `Ctrl+a c`    | Nueva ventana                 |
| `Ctrl+a 1..9` | Ir a la ventana N             |
| `Ctrl+a n / p`| Siguiente / anterior          |
| `Ctrl+a ,`    | Renombrar ventana             |
| `Ctrl+a w`    | Lista de ventanas para elegir |

## 5. Scroll y copiar

| Atajo              | Qué hace                                       |
| ------------------ | ---------------------------------------------- |
| rueda del mouse    | Scroll (entra solo al modo copia)              |
| `Ctrl+a [`         | Modo scroll con teclado (`↑ ↓`, `PgUp`)        |
| `q`                | Salir del modo scroll                          |

> [!note] Copiar texto con el mouse
> Mantén **`Option` (⌥)** mientras seleccionas para usar la selección normal de la terminal y copiar con `Cmd+C`.

## 6. Mi config (`~/.tmux.conf`)

```bash
unbind C-b
set -g prefix C-a
bind C-a send-prefix

set -g mouse on

bind | split-window -h -c "#{pane_current_path}"
bind '\' split-window -h -c "#{pane_current_path}"   # misma tecla sin Shift
bind - split-window -v -c "#{pane_current_path}"
bind c new-window -c "#{pane_current_path}"

bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

set -g base-index 1
setw -g pane-base-index 1
set -g renumber-windows on

set -g default-terminal "tmux-256color"
set -ag terminal-overrides ",xterm-256color:RGB"
set -sg escape-time 0
set -g focus-events on
set -g history-limit 10000

bind r source-file ~/.tmux.conf \; display "Config recargada"

# vim-tmux-navigator: Ctrl+h/j/k/l sin prefijo (ver sección 8)
is_vim="ps -o state= -o comm= -t '#{pane_tty}' | grep -iqE '^[^TXZ ]+ +(\\S+\\/)?g?(view|l?n?vim?x?|fzf)(diff)?$'"
bind -n C-h if-shell "$is_vim" "send-keys C-h" "select-pane -L"
bind -n C-j if-shell "$is_vim" "send-keys C-j" "select-pane -D"
bind -n C-k if-shell "$is_vim" "send-keys C-k" "select-pane -U"
bind -n C-l if-shell "$is_vim" "send-keys C-l" "select-pane -R"
bind -T copy-mode-vi C-h select-pane -L
bind -T copy-mode-vi C-j select-pane -D
bind -T copy-mode-vi C-k select-pane -U
bind -T copy-mode-vi C-l select-pane -R

# Limpiar la pantalla: prefijo + Ctrl+l
bind C-l send-keys C-l

# Redimensionar paneles: prefijo + H/J/K/L (repetible sin volver a presionar el prefijo)
bind -r H resize-pane -L 5
bind -r J resize-pane -D 5
bind -r K resize-pane -U 5
bind -r L resize-pane -R 5

# Redimensionar fino: prefijo + Shift+flecha, de 1 en 1 (repetible)
bind -r S-Left resize-pane -L 1
bind -r S-Down resize-pane -D 1
bind -r S-Up resize-pane -U 1
bind -r S-Right resize-pane -R 1
```

Respaldo de la versión anterior: `~/.tmux.conf.bak`.

Después de editarla: `Ctrl+a r` para recargar.

## 7. Flujo diario (proyecto PYME)

1. Abrir la terminal y entrar a la sesión desde la carpeta del proyecto:
   ```sh
   cd ~/Documents/banhcafe-projects/BANHCAFE-Pyme
   tmux new -A -s pyme
   ```
2. En el primer panel: `nvim .`
3. `Ctrl+a \` → panel a la derecha: `pnpm dev`
4. `Ctrl+a -` → panel abajo: git, `pnpm lint`, etc.
5. `Ctrl+h/j/k/l` para ir y venir entre nvim y las terminales (o clic)
6. `Ctrl+a z` cuando quieras ver el código en pantalla completa
7. `Ctrl+a c` si necesitas otra pestaña; `Ctrl+a 1`, `Ctrl+a 2`… para cambiar
8. Al terminar el día: `Ctrl+a d` → mañana `tmux new -A -s pyme` y todo sigue igual

```
┌──────────────────────┬──────────────┐
│                      │  pnpm dev    │
│       nvim .         ├──────────────┤
│                      │  git / lint  │
└──────────────────────┴──────────────┘
```

> [!tip] Práctica de la primera semana
> Solo memoriza 4 cosas: `tmux new -A -s pyme`, `Ctrl+a \`, `Ctrl+h/j/k/l`, `Ctrl+a d`. El resto con el mouse.

## 8. Mismos atajos que Neovim (vim-tmux-navigator) ✅ instalado

Con **vim-tmux-navigator**, `Ctrl+h/j/k/l` (sin prefijo) mueve entre splits de Neovim y paneles de tmux como si fueran lo mismo.

`~/.config/nvim/lua/plugins/tmux.lua`:

```lua
return {
  {
    "christoomey/vim-tmux-navigator",
    cmd = {
      "TmuxNavigateLeft",
      "TmuxNavigateDown",
      "TmuxNavigateUp",
      "TmuxNavigateRight",
      "TmuxNavigatePrevious",
    },
    keys = {
      { "<c-h>", "<cmd>TmuxNavigateLeft<cr>", desc = "Ir a la izquierda (nvim/tmux)" },
      { "<c-j>", "<cmd>TmuxNavigateDown<cr>", desc = "Ir abajo (nvim/tmux)" },
      { "<c-k>", "<cmd>TmuxNavigateUp<cr>", desc = "Ir arriba (nvim/tmux)" },
      { "<c-l>", "<cmd>TmuxNavigateRight<cr>", desc = "Ir a la derecha (nvim/tmux)" },
    },
  },
}
```

La parte de tmux está en la sección 6.

## 9. Problemas comunes

> [!warning] "Presiono `Ctrl+l` y no pasa nada"
> - **Fuera de tmux** (`echo $TMUX` sale vacío) los atajos de tmux no existen → entra con `tmux new -A -s pyme`.
> - Los atajos con prefijo son **dos pasos**: `Ctrl+a`, soltar, luego la letra. `Ctrl+a+l` todo junto no funciona.
> - Si Neovim ya estaba abierto antes de instalar el plugin: `:qa` y vuelve a abrir `nvim`.
> - Si editaste `~/.tmux.conf`: `Ctrl+a r` para recargarla.

> [!note] Sin plugin, `Ctrl+h/j/k/l` en LazyVim solo se mueve entre ventanas de **Neovim**, no entre paneles de tmux.

Relacionado: [[Terminal]] · [[Atajos Lazyvim]] · [[Lazygit]]
