---
tags:
  - lazyvim
  - neovim
  - atajos
  - busqueda
---

> [!note] Nota
> `Espacio` = tecla líder. Todo en modo Normal (presiona `Esc` antes si estás escribiendo).
> Ver también: [[Atajos Lazyvim]] · [[Nvim en Visual Studio]]

## 1. Buscar un archivo

| Atajo | Qué hace |
|---|---|
| `Espacio Espacio` | Buscar archivo por nombre (como `Cmd + P` en VS Code) |
| `Espacio f r` | Archivos recientes |
| `Espacio ,` | Archivos que ya tienes abiertos |

Escribe solo una parte del nombre: `prodform` encuentra `ProductForm.tsx`.

## 2. Buscar texto en este archivo (Ctrl+F)

| Atajo | Qué hace |
|---|---|
| `/texto` + Enter | Buscar hacia adelante |
| `n` / `N` | Siguiente / anterior coincidencia |
| `*` | Buscar la palabra que está bajo el cursor |
| `Esc` | Quitar el resaltado |
| `Espacio s b` | Lista de todas las líneas del archivo, filtrando mientras escribes |

## 3. Buscar texto en todo el proyecto

| Atajo | Qué hace |
|---|---|
| `Espacio /` | Buscar cualquier texto en el proyecto (como `Cmd + Shift + F`) |
| `Espacio s w` | Buscar la palabra bajo el cursor (o lo que tengas seleccionado) |
| `g r` | Dónde se usa esta variable o función (más preciso que buscar texto) |
| `Espacio s R` | Reabrir la última búsqueda |

## 4. Reemplazar

| Cómo | Qué hace |
|---|---|
| `:%s/viejo/nuevo/g` + Enter | Reemplaza todo en **este archivo** |
| `:%s/viejo/nuevo/gc` + Enter | Igual, pero pregunta en cada uno (`y` sí, `n` no) |
| `Espacio c r` | Renombrar variable/función en todo el proyecto (recomendado para código) |
| `Espacio s r` | Buscar y reemplazar texto en todo el proyecto (panel) |

## 5. Dentro de cualquier buscador

| Tecla | Qué hace |
|---|---|
| Escribir | Filtra los resultados |
| `Ctrl + j` / `Ctrl + k` | Bajar / subir en la lista |
| `Enter` | Abrir |
| `Ctrl + v` | Abrir dividido a la derecha |
| `Esc` | Cerrar |

## Resumen

- Archivo por nombre: `Espacio Espacio`
- Texto en este archivo: `/texto`, luego `n` / `N`
- Texto en el proyecto: `Espacio /`
- Palabra bajo el cursor en el proyecto: `Espacio s w`
- Dónde se usa: `g r`
