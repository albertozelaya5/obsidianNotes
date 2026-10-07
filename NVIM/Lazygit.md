---
tags: [git, lazygit, neovim, atajos]
tema: Lazygit - git visual en la terminal
---
Interfaz visual de git en la terminal: ves archivos cambiados, ramas y commits, y todo se hace con **una tecla**. Ya viene integrado en LazyVim.

> [!info] Cómo abrirlo
> - Desde LazyVim: **`<leader>gg`** (espacio → g → g), en la raíz del proyecto
> - Desde la terminal: `lazygit`
> - Salir: **`q`**

> [!tip] La tecla salvavidas
> **`?`** muestra todos los atajos del panel en el que estás. Si te pierdes, usa `?`.

## 1. La pantalla

```
┌ 1 Status ──┐┌──────────────────────┐
├ 2 Files ───┤│                      │
│            ││   Vista previa /     │
├ 3 Branches ┤│   diff del elemento  │
├ 4 Commits ─┤│   seleccionado       │
├ 5 Stash ───┤│                      │
└────────────┘└──────────────────────┘
```

| Atajo             | Qué hace                             |
| ----------------- | ------------------------------------ |
| `1` … `5`         | Saltar directo a un panel            |
| `h / l` o `Tab`   | Panel anterior / siguiente           |
| `j / k` o flechas | Moverse dentro de la lista           |
| `Enter`           | Entrar al detalle (archivo, commit…) |
| `Esc`             | Volver / cancelar                    |
| `/`               | Buscar / filtrar en la lista         |
| `+` / `_`         | Agrandar / achicar la vista          |

## 2. Lo básico: hacer un commit y subirlo

**Panel 2 · Files**

| Atajo     | Qué hace                                  |
| --------- | ----------------------------------------- |
| `espacio` | Stage / unstage del archivo (`git add`)   |
| `a`       | Stage / unstage de **todo**               |
| `c`       | **Commit** (escribes el mensaje y `Enter`) |
| `A`       | Amend: agregar al último commit           |
| `d`       | Descartar cambios del archivo ⚠️          |
| `e`       | Abrir el archivo en el editor             |

**Desde cualquier panel**

| Atajo | Qué hace       |
| ----- | -------------- |
| `P`   | **Push** (mayúscula) |
| `p`   | **Pull** (minúscula) |
| `f`   | Fetch          |

> [!important] Flujo diario
> `<leader>gg` → `2` → `espacio` en los archivos (o `a` para todos) → `c` → mensaje → `Enter` → `P` → `q`

## 3. Stage de solo una parte del archivo

1. En **Files**, `Enter` sobre el archivo → se abre el diff
2. Moverse con `j / k` y `espacio` para hacer stage de **esa línea / bloque**
3. `v` para seleccionar varias líneas y `espacio`
4. `Esc` para volver

Útil para separar cambios en commits distintos.

## 4. Ramas (Panel 3 · Branches)

| Atajo     | Qué hace                                 |
| --------- | ---------------------------------------- |
| `espacio` | **Cambiar** a esa rama (checkout)        |
| `n`       | Nueva rama desde la actual               |
| `M`       | Merge de la rama seleccionada en la actual |
| `r`       | Rebase de la actual sobre la seleccionada |
| `R`       | Renombrar rama                           |
| `d`       | Borrar rama                              |
| `-`       | Volver a la rama anterior                |

## 5. Commits (Panel 4 · Commits)

| Atajo   | Qué hace                                  |
| ------- | ----------------------------------------- |
| `Enter` | Ver los archivos que cambió el commit     |
| `r`     | Cambiar el mensaje (reword)               |
| `s`     | Squash: unir con el commit de abajo       |
| `d`     | Eliminar commit ⚠️                        |
| `g`     | Opciones de reset (soft / mixed / hard)   |
| `C`     | Copiar commit (cherry-pick); luego `V` para pegar |

> [!warning] Commits ya subidos
> `r`, `s` y `d` reescriben la historia. Hazlo solo en commits que **no** hayas hecho push, o tendrás que forzar el push.

## 6. Stash (guardar cambios temporalmente)

| Atajo             | Qué hace                                  |
| ----------------- | ----------------------------------------- |
| `s` (en Files)    | Guardar todos los cambios en el stash     |
| `5` → `espacio`   | Aplicar el stash seleccionado             |
| `5` → `g`         | Pop (aplicar y borrar)                    |
| `5` → `d`         | Borrar stash                              |

Útil cuando tienes que cambiar de rama sin hacer commit.

## 7. Deshacer

| Atajo | Qué hace                                          |
| ----- | ------------------------------------------------- |
| `z`   | **Deshacer** la última acción (commit, checkout…) |
| `Z`   | Rehacer                                           |

> [!note] `z` no deshace los descartes
> Lo que borras con `d` en Files **no** se recupera. Úsalo con cuidado.

### Deshacer el último commit

**Si todavía NO hiciste push:**

1. `4` para ir a **Commits**
2. Bajar con `j` al commit **anterior** al que quieres deshacer (el segundo de la lista)
3. `g` → elegir el tipo de reset:

| Opción    | Qué pasa con los cambios del commit                 |
| --------- | --------------------------------------------------- |
| **soft**  | Vuelven a Files **en stage**, listos para otro commit (la más usada) |
| **mixed** | Vuelven a Files **sin stage**                       |
| **hard**  | Se **borran** ⚠️ no se recuperan                    |

Equivale a `git reset --soft HEAD~1` en la terminal.

> [!tip] Atajo rápido
> Si el commit fue lo **último** que hiciste en lazygit, basta con `z`.
> Si solo querías corregir el mensaje o agregar un archivo, no hace falta deshacerlo: usa `r` (reword) en Commits o `A` (amend) en Files.

**Si YA hiciste push:**

En **Commits**, sobre el commit, presiona `t` (**revert**). Crea un commit nuevo que anula los cambios sin reescribir la historia. Luego haz `P`.

> [!warning] No uses reset en commits ya subidos
> Te obliga a forzar el push y puede romperle la rama a tus compañeros. Usa revert.

## 8. Chuleta mínima (para memorizar primero)

| Tecla     | Acción              |
| --------- | ------------------- |
| `espacio` | stage               |
| `a`       | stage todo          |
| `c`       | commit              |
| `P` / `p` | push / pull         |
| `3` + `espacio` | cambiar de rama |
| `z`       | deshacer            |
| `?`       | ayuda               |
| `q`       | salir               |

## 9. Con tmux

Si prefieres tenerlo aparte del editor: `Ctrl+a c` (nueva ventana de tmux) → `lazygit`, y cambias con `Ctrl+a 1` / `Ctrl+a 2`. Ver [[Tmux]].

Relacionado: [[Atajos Lazyvim]] · [[Tmux]]
