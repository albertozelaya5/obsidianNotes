---
tags:
  - lazyvim
  - neovim
  - atajos
  - busqueda
---

> [!note] Nota
> `Espacio` = `<leader>` en LazyVim. Todos los atajos son en modo normal.
> El buscador que usa tu LazyVim es **Snacks picker**.
> Ver también: [[Atajos Lazyvim]] · [[Nvim en Visual Studio]]

## Explorador de archivos

| Atajo | Qué hace |
|---|---|
| `Espacio e` | Abrir / cerrar el explorador (raíz del proyecto) |
| `Espacio E` | Explorador en la carpeta actual (cwd) |
| `/` (dentro del explorador) | Filtrar archivos |

## Buscar archivos por nombre

| Atajo | Qué hace |
|---|---|
| `Espacio Espacio` | Buscar archivo por nombre (raíz del proyecto) |
| `Espacio f f` | Lo mismo que `Espacio Espacio` |
| `Espacio f F` | Buscar archivo en la carpeta actual (cwd) |
| `Espacio f g` | Buscar solo archivos de git |
| `Espacio f r` | Archivos recientes |
| `Espacio f R` | Archivos recientes (solo de este proyecto) |
| `Espacio ,` o `Espacio f b` | Archivos abiertos (buffers) |
| `Espacio f c` | Archivos de configuración de nvim |

## Buscar texto / palabras

| Atajo | Qué hace |
|---|---|
| `Espacio /` | Buscar texto en todo el proyecto (grep) |
| `Espacio s g` | Lo mismo que `Espacio /` |
| `Espacio s G` | Grep en la carpeta actual (cwd) |
| `Espacio s w` | Buscar la palabra bajo el cursor (o lo seleccionado en visual) en el proyecto |
| `Espacio s b` | Buscar líneas dentro del archivo actual |
| `Espacio s B` | Buscar texto solo en los archivos abiertos |
| `/` y `?` | Buscar en el archivo actual hacia adelante / atrás (`n` / `N` para moverte) |
| `*` / `#` | Buscar la palabra bajo el cursor en el archivo |
| `Espacio s r` | Buscar y reemplazar en todo el proyecto (grug-far) |

## Buscar en el código (LSP)

| Atajo | Qué hace |
|---|---|
| `g d` | Ir a la definición |
| `g r` | Ver dónde se usa (referencias) |
| `g I` | Ir a la implementación |
| `g y` | Ir a la definición del tipo |
| `Espacio s s` | Símbolos del archivo (funciones, variables, componentes) |
| `Espacio s S` | Símbolos de todo el proyecto |
| `Espacio s d` | Errores / diagnósticos del proyecto |
| `Espacio s D` | Errores del archivo actual |

## Otros buscadores útiles

| Atajo | Qué hace |
|---|---|
| `Espacio s R` | Reabrir la última búsqueda (resume) |
| `Espacio s k` | Buscar atajos (keymaps) |
| `Espacio s c` | Historial de comandos |
| `Espacio s C` | Lista de comandos |
| `Espacio s h` | Ayuda de nvim |
| `Espacio s j` | Lista de saltos (jumps) |
| `Espacio s m` | Marcas |
| `Espacio s t` | TODOs del proyecto |
| `Espacio s u` | Historial de deshacer (undotree) |
| `Espacio n` | Historial de notificaciones |
| `Espacio g s` | Git status |
| `Espacio g d` | Git diff (cambios) |

## Dentro del buscador

| Atajo | Qué hace |
|---|---|
| `Enter` | Abrir |
| `Ctrl + v` | Abrir dividido a la derecha |
| `Ctrl + s` | Abrir dividido abajo |
| `Ctrl + j` / `Ctrl + k` | Bajar / subir en la lista |
| `Tab` | Marcar varios resultados |
| `Ctrl + q` | Mandar resultados a la lista quickfix |
| `Alt + h` | Mostrar / ocultar archivos ocultos |
| `Alt + i` | Mostrar / ocultar archivos ignorados (node_modules, etc.) |
| `Alt + p` | Mostrar / ocultar la vista previa |
| `Esc` | Cerrar |

## Resumen rápido

- Explorador: `Espacio e`
- Archivo por nombre: `Espacio Espacio`
- Texto en el proyecto: `Espacio /`
- Palabra bajo el cursor: `Espacio s w`
- Dónde se usa: `g r`
- Archivos abiertos: `Espacio ,`
- Recientes: `Espacio f r`

> [!tip] Tip
> Presiona `Espacio` (o `Espacio s`, `Espacio f`) y espera: which-key te muestra todos los atajos de ese grupo.
