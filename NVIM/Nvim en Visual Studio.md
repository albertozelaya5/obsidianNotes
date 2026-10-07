---
tags: [neovim, lazyvim, explorador, ventanas]
tema: Explorador y ventanas divididas en LazyVim
---
## 1. Cambiar el enfoque entre explorador y archivo

El explorador es una ventana más, así que se usan los atajos de moverse entre ventanas.

| Atajo      | Qué hace                         |
| ---------- | -------------------------------- |
| `Ctrl+h`   | Ir al explorador (izquierda)     |
| `Ctrl+l`   | Regresar al archivo (derecha)    |
| `Ctrl+w w` | Alternar entre ventanas en ciclo |

> [!tip] Abrir desde el explorador
> Al presionar `Enter` sobre un archivo en el explorador, se abre y el enfoque pasa automáticamente a él.

## 2. Poner archivos lado a lado

### Desde el explorador o el buscador

| Atajo | Qué hace |
|---|---|
| `Ctrl+v` | Abre el archivo dividido a la derecha (vertical) |
| `Ctrl+s` | Abre el archivo dividido abajo (horizontal) |

> [!note] Funciona en ambos
> Sirve tanto en el explorador (`Espacio e`) como en el buscador de archivos (`Espacio Espacio`): en vez de `Enter`, presiona `Ctrl+v`.

### Dividiendo la ventana actual

| Atajo | Qué hace |
|---|---|
| `Espacio \|` | Divide la ventana a la derecha |
| `Espacio -` | Divide la ventana abajo |
| `Espacio w d` | Cierra la división actual |

Moverse entre las divisiones: `Ctrl+h` / `Ctrl+j` / `Ctrl+k` / `Ctrl+l`

## 3. Buscar dentro del explorador

1. Ir al explorador con `Ctrl+h`
2. Presionar `/` → el cursor salta al campo de búsqueda
3. Escribir para filtrar archivos
4. `Esc` o `/` otra vez → regresar a la lista

## Resumen rápido

- Ir al explorador: `Ctrl+h`
- Volver al archivo: `Ctrl+l`
- Abrir archivo al lado: `Ctrl+v`
- Buscar en el explorador: `/`
- Cerrar división: `Espacio w d`