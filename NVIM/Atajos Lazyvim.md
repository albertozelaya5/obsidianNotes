---
tags:
  - lazyvim
  - neovim
  - atajos
---

> [!note] Cómo leer esta nota
> `Espacio` es la tecla **líder** de LazyVim. `Espacio e` = presiona Espacio, suéltalo y luego `e`.
> Todo es en **modo Normal**. Si te pierdes, presiona `Esc`.
> ¿Olvidaste un atajo? Presiona `Espacio` y **espera**: aparece un menú con todos.
> Más detalle: [[Atajos de búsqueda]] · [[Nvim en Visual Studio]] · [[Tmux]]

## 1. Abrir y salir

| Comando / atajo | Qué hace |
|---|---|
| `nvim .` | Abre nvim en la carpeta actual (escríbelo en la terminal) |
| `Ctrl + s` | Guardar (también funciona escribiendo) |
| `:w` + Enter | Guardar |
| `Espacio q q` | Salir de nvim (te avisa si hay cambios sin guardar) |
| `:q!` + Enter | Salir **sin guardar** |

## 2. Los modos (lo mínimo)

| Tecla | Modo | Para qué |
|---|---|---|
| `i` | Insertar | Escribir texto |
| `Esc` | Normal | Moverte y dar órdenes (también quita el resaltado de búsqueda) |
| `v` / `V` | Visual | Seleccionar letras / líneas completas |
| `:` | Comando | Escribir órdenes como `:w` |

## 3. Explorador de archivos

| Atajo | Qué hace |
|---|---|
| `Espacio e` | Abrir / cerrar el explorador |
| `Ctrl + h` | Pasar el foco **al explorador** (izquierda) |
| `Ctrl + l` | Volver **al código** (derecha) |

Dentro del explorador:

| Tecla         | Qué hace                                          |
| ------------- | ------------------------------------------------- |
| `j` / `k`     | Bajar / subir                                     |
| `Enter` o `l` | Abrir archivo o carpeta                           |
| `h`           | Cerrar carpeta                                    |
| `a`           | Crear archivo (termina en `/` para crear carpeta) |
| `r`           | Renombrar                                         |
| `d`           | Borrar                                            |
| `/`           | Filtrar por nombre                                |
| `H`           | Mostrar / ocultar archivos ocultos (`.env`…)      |
| `?`           | Ver todas las teclas del explorador               |
| `I`           | Ignorados por git (.env, node_modules, dist…)     |

## 4. Archivos abiertos (las "pestañas" de arriba)

| Atajo | Qué hace |
|---|---|
| `Shift + l` / `Shift + h` | Siguiente / anterior archivo abierto |
| `Espacio b b` | Volver al archivo anterior (ir y venir entre dos) |
| `Espacio ,` | Lista de archivos abiertos para elegir |
| `Espacio b d` | **Cerrar el archivo actual** |
| `Espacio b o` | Cerrar todos menos el actual |

## 5. Buscar (lo más usado)

| Atajo             | Qué hace                                                                   |
| ----------------- | -------------------------------------------------------------------------- |
| `Espacio Espacio` | Buscar **archivo por nombre**                                              |
| `Espacio /`       | Buscar **texto en todo el proyecto**                                       |
| `Espacio s w`     | Buscar en el proyecto **la palabra bajo el cursor**                        |
| `/texto` + Enter  | Buscar en **este archivo** (como Ctrl+F); `n` / `N` = siguiente / anterior |
| `Espacio f r`     | Archivos recientes                                                         |

Todo lo de búsqueda está en [[Atajos de búsqueda]].

## 6. Buscar componentes -  Moverse con el codigo

| Atajo       | Qué hace                                                |
| ----------- | ------------------------------------------------------- |
| `g d`       | Ir a la **definición** de lo que está bajo el cursor    |
| `g r`       | Ver **dónde se usa**                                    |
| `K`         | Ver info / tipo (como pasar el mouse encima en VS Code) |
| `Ctrl + o`  | **Regresar** a donde estabas antes de saltar            |
| `g g` / `G` | Ir al inicio / final del archivo                        |

## 7. Editar código

| Atajo | Qué hace |
|---|---|
| `u` / `Ctrl + r` | Deshacer / rehacer |
| `Espacio c r` | **Renombrar** variable o función en todo el proyecto |
| `Espacio c a` | Acciones rápidas (importar, arreglar error…), como el foco amarillo de VS Code |
| `Espacio c f` | Formatear el archivo (Prettier) |
| `] d` / `[ d` | Ir al siguiente / anterior error o aviso |

**Cambiar una palabra una por una:** cursor en la palabra → `*` → `cgn` → escribe → `Esc` → `.` repite en la siguiente (`n` para saltarla).

**Escribir en varias líneas a la vez:** `Ctrl + v` → baja con `j` → `Shift + i` → escribe → `Esc`.

## 8. Comentar código

| Atajo              | Qué hace                                   |
| ------------------ | ------------------------------------------ |
| `g c c`            | Comentar / descomentar **la línea actual** |
| `V` → `j`… → `g c` | Comentar **varias líneas** seleccionadas   |
| `5 g c c`          | Comentar **5 líneas** desde el cursor      |
| `g c a p`          | Comentar un **bloque** (párrafo, función)  |

## 9. Ventanas (archivos lado a lado)

| Atajo | Qué hace |
|---|---|
| `Espacio \|` | Dividir a la derecha |
| `Espacio -` | Dividir abajo |
| `Ctrl + h / j / k / l` | Moverte entre divisiones (← ↓ ↑ →) |
| `Espacio w d` | Cerrar la división actual |

> [!warning] Cerrar archivo ≠ cerrar ventana
> `Espacio b d` cierra el **archivo**. `Espacio w d` cierra la **división**, y el archivo sigue abierto.
> Si solo tienes una ventana, `Espacio w d` no hace nada útil.

## 10. Git y terminal

| Atajo | Qué hace |
|---|---|
| `Espacio g g` | Abrir **lazygit** (`q` para salir) |
| `Ctrl + /` | Abrir / cerrar una terminal flotante |

## 11. Practicar con nvim "puro" (sin LazyVim)

Tu config de LazyVim **desactiva el tutorial**, así que `nvim +Tutor` da error. Usa:

| Comando (en la terminal) | Qué hace |
|---|---|
| `nvim --clean +Tutor` | Abre el **tutorial interactivo** en nvim sin plugins (~30 min) |
| `nvim --clean` | nvim vanilla, sin tu config (para practicar o ver si algo falla por un plugin) |
| `vimtutor` | Tutorial clásico de Vim (alternativa) |

Dentro del tutorial, `:Tutor` lista las lecciones y `:q!` sale.
