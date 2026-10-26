---
tags:
  - lazyvim
  - neovim
  - atajos
---

> [!note] Nota
> `<leader>` = Barra espaciadora (por defecto en LazyVim).
> Todos los atajos son en modo normal, salvo que se indique otro modo.

## Moverse entre pestañas (buffers)

Las "pestañas" de arriba en LazyVim son **buffers** (archivos abiertos).

| Atajo | Acción |
|---|---|
| `Shift + l` (L) | Siguiente buffer |
| `Shift + h` (H) | Buffer anterior |
| `]b` / `[b` | Siguiente / anterior (alternativa) |
| `<leader>bb` | Saltar al último buffer usado |
| `<leader>,` o `<leader>fb` | Buscador con la lista de buffers |

### Pestañas reales de Vim (tab pages)

| Atajo | Acción |
|---|---|
| `gt` / `gT` | Siguiente / anterior pestaña |
| `<leader><tab>]` / `<leader><tab>[` | Siguiente / anterior pestaña |

> [!tip] Tip
> Presiona `Espacio` y espera: el menú de which-key muestra todos los atajos disponibles.

## Buscar palabras en común (variables, estados, etc.)

### Ver y navegar en el archivo actual

| Atajo | Acción |
|---|---|
| `*` / `#` | Buscar la palabra bajo el cursor adelante / atrás y resaltarlas |
| `n` / `N` | Siguiente / anterior coincidencia |
| `]]` / `[[` | Siguiente / anterior referencia resaltada |
| `Esc` o `:noh` | Quitar el resaltado |

### Ver dónde se usa en todo el proyecto

| Atajo | Acción |
|---|---|
| `gr` | Lista de referencias (LSP) |
| `gd` | Ir a la definición |
| `<leader>sw` | Buscar la palabra bajo el cursor en el proyecto |
| `<leader>/` | Buscar cualquier texto en el proyecto (grep) |

## Editar todas a la vez

- `<leader>cr` → Renombrar con LSP en todos los archivos (lo mejor para variables, funciones, estados, props).
- `<leader>sr` → Buscar y reemplazar en todo el proyecto (panel grug-far). En **Flags** pon `-i` para ignorar mayúsculas.

### En el archivo actual

**Forma 1: con el asterisco** (usa la palabra bajo el cursor)

1. Cursor en la palabra → `*`
2. `:%s//nuevoNombre/g`

**Forma 2: escribiendo la palabra directamente**

```vim
:%s/palabra/nueva/g
```

| Opción | Qué hace |
|---|---|
| `/g` | Reemplaza todas las coincidencias exactas |
| `/gi` | Ignora mayúsculas y minúsculas (`palabra`, `Palabra`, `PALABRA`) |
| `/gc` | Pide confirmación en cada cambio (`y` sí, `n` no) |
| `/gic` | Ignora mayúsculas y pide confirmación |

> [!warning] Cuidado
> Usa `%` (todo el archivo), no `$` (solo la última línea).
> En código evita `/gi`: puede cambiar cosas distintas como `queryClient` y `QueryClient`.

### Editar o borrar una por una

**Cambiar:**

1. Cursor en la palabra → `*`
2. `cgn` → escribir el texto nuevo → `Esc`
3. `.` para repetir en la siguiente, `n` para saltarla.

**Borrar:**

1. Cursor en la palabra → `*`
2. `dgn`
3. `.` para repetir, `n` para saltar.

## Editar varias líneas a la vez

Se hace con el **modo visual de bloque** (`Ctrl + v`).

**Escribir al inicio de varias líneas:**

1. `Ctrl + v` → bajar con `j` para seleccionar las líneas
2. `Shift + i` (I) → escribir el texto → `Esc`
3. El texto aparece en todas las líneas al presionar `Esc`.

**Escribir al final de varias líneas:**

1. `Ctrl + v` → bajar con `j`
2. `$` → `Shift + a` (A) → escribir → `Esc`

**Borrar una columna de texto:**

1. `Ctrl + v` → seleccionar con `j` / `l`
2. `d`

**Comentar varias líneas:**

1. `V` (V mayúscula) → seleccionar con `j`
2. `gc`

| Atajo | Acción |
|---|---|
| `Ctrl + v` | Modo visual de bloque (columnas) |
| `V` | Seleccionar líneas completas |
| `I` / `A` | Insertar al inicio / al final (en bloque) |
| `gc` | Comentar / descomentar la selección |
| `gcc` | Comentar / descomentar la línea actual |

## Recomendación

> [!tip] Recomendación
> Para código (ej. un estado de React): usa `gr` para ver dónde se usa y `<leader>cr` para renombrarlo. El LSP entiende el código y no toca palabras iguales sin relación.
> Para texto normal: `:%s/palabra/nueva/g`.
> Para agregar algo en muchas líneas: `Ctrl + v` + `I` o `A`.

commit bbe95e549a6ef9672bf44b05f44a31170d8f1d22 (HEAD -> fix/comparer-core-req-341)
Author: albertozelaya <ajzelaya@banhcafe.hn>
Date:   Wed Sep 23 11:56:32 2026 -0600

    feat: add modal ModalFileTimeline, and delete multiple fields

commit 6c747644af8ffa0ad84ce6777b57b8585e76a311
Author: albertozelaya <ajzelaya@banhcafe.hn>
Date:   Wed Sep 23 10:55:00 2026 -0600

    feat: add new no correlative screen

commit f67d55c89f2213c151d65449cb8fc524bf685cd0
Merge: b9ac80d84 07428cef2
Author: albertozelaya <ajzelaya@banhcafe.hn>
Date:   Wed Sep 23 10:53:23 2026 -0600

    Merge branch 'master' into fix/comparer-core-req-341

commit b9ac80d84b5c3b98de2801243585bf196b393f23 (origin/fix/comparer-core-req-341)
Author: albertozelaya <ajzelaya@banhcafe.hn>
Date:   Thu Sep 17 11:01:13 2026 -0600

    feat: add statusFileId

commit 07428cef23b4dd73e924dee7e67019afa7f518b2 (origin/master, origin/HEAD, master)
Merge: 6b79ebaa7 53911ff4f
Author: Alberto Javier Zelaya Matamoros <ajzelaya@banhcafe.hn>
Date:   Mon Sep 14 22:02:38 2026 +0000

    Merged PR 979: feat: Accordion optional pagination (local + server-side)
    
    When paginated is omitted the component behaves exactly as before. The
    original panel rendering + selectedItems state is extracted unchanged into
    AccordionPanels and remounted per page via key.
    
    Related work items: #152