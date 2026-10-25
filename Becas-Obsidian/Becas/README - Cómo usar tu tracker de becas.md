---
sources: cowork
---

# Tracker de becas en Obsidian (con Bases)

Esto replica tu tabla de Notion, pero usando la función nativa **Bases** de Obsidian: cada beca es una nota individual con propiedades (país, opción, etapa, deadline, enlaces...), y `Becas.base` las junta en una vista tipo tabla — con **filtros y "selects" reales**, no una tabla de texto donde escribes todo a mano.

## 1. Instalar en tu vault

1. Copia la carpeta **`Becas/`**, el archivo **`Becas.base`** y **`Plantilla - Nueva beca.md`** a la raíz de tu vault de Obsidian (o a la carpeta donde quieras tenerlos — si los mueves, edita la línea `file.inFolder("Becas")` dentro de `Becas.base` para que apunte a la ruta correcta).
2. Necesitas **Obsidian 1.9 o más reciente**. Ve a *Configuración → Plugins base → Bases* y confirma que esté activado (viene activado por defecto en versiones recientes).
3. Haz clic en `Becas.base` en el explorador de archivos para abrir la vista. Verás varias pestañas arriba: **Todas las postulaciones**, **Por etapa**, **Próximos vencimientos**, **Por aplicar**, **Nuevas (sin revisar)** y **Favoritos** — el equivalente a las pestañas "All applications / By stage / Due dates..." de tu Notion.

## 2. Cómo funciona lo de "seleccionar en vez de escribir"

Obsidian no tiene un tipo de propiedad "select" idéntico a Notion, pero se comporta muy parecido:

- **`pais`, `opcion`, `etapa`**: son propiedades de texto. Cuando haces clic en la celda para editarla (ya sea desde la tabla de Bases o desde las propiedades de la nota), Obsidian te sugiere automáticamente los valores que ya usaste antes en esa misma propiedad en otras notas — así que después de las primeras becas, solo escribes una letra y seleccionas de la lista en vez de tipear todo. Para reforzar esto, en cualquier nota abre las propiedades, haz clic en el ícono a la izquierda de `pais` (o `opcion`/`etapa`) → **Cambiar tipo** → **Lista**. Con tipo Lista se ve igual que las etiquetas de colores de Notion, con burbujas seleccionables.
- **`deadline`**: cámbiala a tipo **Fecha** (mismo menú: clic en el ícono → Cambiar tipo → Fecha) y te va a abrir un calendario para escoger el día, en vez de escribir la fecha a mano.
- **`favorito`**: ya viene como casilla (checkbox) — se ve como el ❤️ de tu Notion, haces clic y ya.
- **`link` / `info_aplicacion`**: estos sí quedan como texto libre, porque son URLs distintas para cada beca (no tiene sentido "seleccionarlas" de una lista).

Como el tipo de propiedad se define una sola vez y aplica a todo el vault, con hacer este ajuste en **una sola nota** (por ejemplo la plantilla) ya queda así para las 27 notas.

## 3. Valores sugeridos (tu "leyenda")

Para que el autocompletado funcione bien, usa siempre los mismos valores. Estos son los que ya vienen precargados desde tu tabla de Notion:

**`opcion`**: `To Consider` · `Best options` · `Backup Option`

**`etapa`**: `Ready to apply` · `No Aplica` · `Por revisar` (agregué este último para las becas nuevas que aún no has evaluado — puedes cambiarlo por `Ready to apply` o `No Aplica` en cuanto las revises)

**`pais`**: usa el nombre del país tal cual (Alemania, España, Italia, Australia, Rusia, Corea, Holanda, Suiza, Rumania, Europa...) — igual que en tu tabla original.

## 4. Qué ya viene cargado

Cargué las **26 becas nuevas** de la lista que me pasaste (del boletín de INDESGUA), una nota por beca, con el país ya completado a partir del nombre. Los campos `opcion`, `deadline`, `link`, `info_aplicacion` y `notas` quedaron en blanco porque no venían en tu lista de texto — solo tienes que abrir cada nota y completarlos (o desde la tabla de Bases directamente, haciendo clic en cada celda). Las dos marcadas `[NUEVO]` en tu lista original (Skolkovo/Skoltech y Universidad de Valencia) quedaron con `nuevo: true`, así aparecen en la pestaña "Nuevas (sin revisar)".

No re-transcribí las ~20 becas que ya tenías con datos completos en tu tabla de Notion (Fulbright, Palermo, Paris Saclay, Milán, Bolonia, etc.) porque en la captura de pantalla los enlaces aparecen cortados (con "...") y no quise inventar URLs que podrían estar mal. Lo más rápido para pasarlas: duplica `Plantilla - Nueva beca.md` (Ctrl/Cmd + D o "Hacer una copia"), ponle el nombre de la beca, y copia/pega los datos reales desde tu Notion.

## 5. Agregar una beca nueva

1. Duplica **`Plantilla - Nueva beca.md`** (clic derecho → *Hacer una copia*) y muévela a la carpeta `Becas/`.
2. Cambia el nombre del archivo por el de la beca — ese nombre es lo que se muestra como "Opportunity" en la tabla.
3. Llena las propiedades arriba del archivo (o hazlo directo desde la fila nueva en `Becas.base`, con el botón **+** al final de la tabla).

## 6. Ordenar por fecha límite

En la pestaña "Próximos vencimientos", haz clic en el encabezado de la columna **deadline** (o en el botón de ordenar de la barra superior de Bases) para ordenar ascendente por fecha — Obsidian guarda esa preferencia de orden automáticamente dentro del archivo `.base`.
