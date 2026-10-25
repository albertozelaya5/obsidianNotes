Orden recomendado para entender el stack de **AgriTech WebSite** (TanStack Start + Router + Query + shadcn/ui + Radix) en el menor tiempo posible, partiendo de que ya sabés React. **Total estimado: ~2–3 días de estudio + 1 fin de semana de práctica.**

Notas involucradas: [[Shadcn UI]] · [[React Query (Tanstack query)]] · [[TanStack Router y Start]]

> [!tip] Cómo usar esta nota
> 1. Leé las secciones indicadas de la nota.
> 2. Leé los archivos reales de AgriTech y marcalos.
> 3. Respondé las **preguntas de autoevaluación** en voz alta o por escrito **antes** de abrir la respuesta (están plegadas, hacé clic para verlas).
> 4. Si fallás más de una, volvé a la sección antes de pasar al siguiente paso.

---

## Paso 1 — shadcn/ui + Radix (≈ 2–3 h)

📖 [[Shadcn UI]]: secciones 1 a 9 (qué es, `components.json`, tokens, `cva`, `cn()`, `asChild`, editar in-place).

- Saltá por ahora: sección 10 (Form + React Hook Form) y 14 (Charts). AgriTech **no** usa React Hook Form en el LeadForm.
- Radix en 3 ideas: composición por partes (`Dialog` / `DialogTrigger` / `DialogContent`), `asChild`, atributos `data-state`.

🔍 Leé en AgriTech:
- [ ] `src/components/ui/button.tsx` (cva + variantes)
- [ ] `src/components/ui/dialog.tsx` (Radix envuelto)
- [ ] `src/lib/utils.ts` (`cn`)
- [ ] `src/styles.css` (tokens: `bg-primary`, `text-foreground`, escala `text-14`…)

### ✅ Autoevaluación

**1.** Si querés cambiar el `border-radius` de todos los botones del proyecto, ¿actualizás un paquete en `package.json` o qué hacés?

> [!question]- Respuesta
> Editás directamente `src/components/ui/button.tsx`. shadcn **no es un paquete**: el CLI copió el código a tu repo y sos dueño de él.

**2.** ¿Qué hace cada pieza: Radix, Tailwind, shadcn?

> [!question]- Respuesta
> **Radix** = comportamiento + accesibilidad (foco, teclado, ARIA, portales), sin estilos. **Tailwind** = estilos. **shadcn** = el código que une ambos y te lo deja en `components/ui/`.

**3.** ¿Qué problema resuelve `cn("px-4 bg-primary", className)` que no resuelve concatenar strings?

> [!question]- Respuesta
> `cn` = `clsx` + `tailwind-merge`. Si `className` trae `px-8`, **gana `px-8`** y se elimina `px-4`. Con concatenación quedarían las dos clases y ganaría la que esté después en el CSS, no la que vos pasaste.

**4.** ¿Qué pasa con `<DialogTrigger asChild><Button>Abrir</Button></DialogTrigger>`? ¿Y sin `asChild`?

> [!question]- Respuesta
> Con `asChild`, el trigger **no crea su propio `<button>`**: le pasa sus props y eventos al hijo (`Button`). Sin `asChild` tendrías un `<button>` dentro de otro `<button>` (HTML inválido).

**5.** En `button.tsx`, ¿dónde agregarías una variante `variant="campo"` con fondo verde? ¿Y de dónde sale el color?

> [!question]- Respuesta
> En el objeto `variants.variant` del `cva(...)`. El color sale de un **token** de `src/styles.css` (ej. `bg-success`), **nunca** un hex ni `bg-green-600`.

**6.** ¿Cómo estilás un componente Radix distinto cuando está abierto?

> [!question]- Respuesta
> Radix pone `data-state="open"` / `"closed"` en el elemento. En Tailwind: `data-[state=open]:rotate-180`, `data-[state=open]:animate-in`, etc.

---

## Paso 2 — React Query (≈ 2–3 h)

📖 [[React Query (Tanstack query)]]: secciones 1 a 9 (server state, `useQuery`, keys, `staleTime`, `useMutation`, `invalidateQueries`).

- Lo vas a necesitar **antes** del Router porque el paso 4 los une.

🔍 Leé en AgriTech:
- [ ] `src/router.tsx` (dónde nace el `QueryClient`)

### ✅ Autoevaluación

**1.** ¿Cuál es la diferencia entre *server state* y *client state*? Dá un ejemplo de cada uno en AgriTech.

> [!question]- Respuesta
> **Server state**: datos que viven en un servidor y pueden cambiar sin que vos hagas nada (lista de proveedores, posts del blog si vinieran de una API). **Client state**: vive solo en el navegador (si el menú mobile está abierto, el monto que escribiste en la Calculadora).

**2.** Dos componentes llaman `useQuery({ queryKey: ["providers"], ... })` al mismo tiempo. ¿Cuántas requests se hacen?

> [!question]- Respuesta
> **Una.** Misma key = misma entrada de caché; React Query deduplica.

**3.** ¿Por qué `["providers", { q }]` y no solo `["providers"]` cuando hay un filtro?

> [!question]- Respuesta
> Porque la key identifica **el dato exacto**. Si no incluís `q`, todos los filtros comparten la misma caché y verías resultados de otra búsqueda.

**4.** Explicá `staleTime` vs `gcTime` en una frase cada uno.

> [!question]- Respuesta
> **`staleTime`**: cuánto tiempo el dato se considera fresco (no se re-pide). **`gcTime`**: cuánto tiempo se guarda en memoria un dato que **ningún componente está usando** antes de borrarlo.

**5.** Creaste un proveedor con `useMutation`. ¿Cómo hacés que la lista se actualice sola?

> [!question]- Respuesta
> En `onSuccess`: `queryClient.invalidateQueries({ queryKey: ["providers"] })`. Marca esas queries como viejas y las que están en pantalla se re-piden.

**6.** ¿Dónde se crea el `QueryClient` en AgriTech y a quién se le pasa?

> [!question]- Respuesta
> En `src/router.tsx`, dentro de `getRouter()`. Se pasa al router como `context: { queryClient }`, así cualquier `loader` puede usarlo.

---

## Paso 3 — TanStack Router (≈ 3–4 h)

📖 [[TanStack Router y Start]]: secciones 1 a 9.

Orden interno sugerido: 2 (nombres de archivo) → 3 (`createFileRoute`) → 4 (`<Link>`) → 5 (params) → 7 (loader) → 6 (search params) → 8 (layouts) → 9 (guards).

🔍 Leé en AgriTech:
- [ ] `src/routes/README.md`
- [ ] `src/routes/index.tsx` (ojo: los `<Hero />;` sueltos son código muerto; lo que se renderiza está en `LandingPage()`)
- [ ] `src/routes/blog.$slug.tsx` (param + loader + `notFound`: **el archivo más didáctico del repo**)
- [ ] `src/components/layout/Navbar.tsx` (links `to` vs `hash`)

### ✅ Autoevaluación

**1.** ¿Qué URL genera cada archivo? `contacto.tsx` · `cultivos.$id.tsx` · `cultivos.index.tsx` · `_auth.perfil.tsx`

> [!question]- Respuesta
> `/contacto` · `/cultivos/:id` · `/cultivos` · `/perfil` (el `_auth` es un layout **sin** path, no aparece en la URL).

**2.** ¿Qué está mal acá y cómo lo corregís? `<Link to={"/blog/" + post.slug}>`

> [!question]- Respuesta
> Se arma la URL con strings y se pierde el tipado. Correcto: `<Link to="/blog/$slug" params={{ slug: post.slug }}>`.

**3.** En `blog.$slug.tsx`, ¿qué pasa si alguien entra a `/blog/no-existe`? Recorré el flujo.

> [!question]- Respuesta
> El `loader` corre antes del render, no encuentra el post y hace `throw notFound()`. Como la ruta no define `notFoundComponent`, se usa el `NotFoundComponent` global de `__root.tsx` (el 404 rojo).

**4.** ¿Por qué guardar un filtro en search params (`?cultivo=cafe`) en vez de `useState`?

> [!question]- Respuesta
> Sobrevive al refresh, se puede compartir el link y el botón "atrás" funciona. Además con `validateSearch` queda tipado.

**5.** Tu `loader` usa `search.q`, cambiás el filtro y los datos no se actualizan. ¿Qué falta?

> [!question]- Respuesta
> `loaderDeps: ({ search }) => ({ q: search.q })`, y en el loader leer `deps.q`. Sin eso el Router no sabe que el loader depende de `q`.

**6.** ¿Qué hace `navigate({ search: (prev) => ({ ...prev, page: 2 }) })` que no hace `navigate({ search: { page: 2 } })`?

> [!question]- Respuesta
> Conserva los otros search params (`q`, `cultivo`…). La segunda forma **los borra** y deja solo `page`.

**7.** ¿Qué es `src/routeTree.gen.ts` y qué hacés si una ruta nueva no aparece?

> [!question]- Respuesta
> Lo genera el plugin a partir de `src/routes/`. **Nunca se edita a mano.** Si no aparece la ruta: guardá el archivo de nuevo o reiniciá `bun run dev`.

**8.** ¿Para qué sirve `beforeLoad` y en qué se diferencia de `loader`?

> [!question]- Respuesta
> `beforeLoad` corre **antes** y sirve para decidir si se puede entrar (`throw redirect(...)`) o para agregar cosas al `context`. `loader` es para **traer los datos** de la página.

---

## Paso 4 — Router + Query juntos (≈ 1 h)

📖 [[TanStack Router y Start]]: sección 10 (`queryOptions` → `ensureQueryData` → `useSuspenseQuery`).

🔍 Leé en AgriTech:
- [ ] `src/routes/__root.tsx`: `createRootRouteWithContext<{ queryClient }>`

### ✅ Autoevaluación

**1.** ¿Por qué se define la query con `queryOptions(...)` en un solo lugar en vez de escribir la key y la `queryFn` dos veces?

> [!question]- Respuesta
> Porque la usan **dos** lugares (loader y componente) y tienen que tener **exactamente la misma key**. Si no coinciden, el componente no encuentra la caché y vuelve a pedir.

**2.** ¿Qué hace `ensureQueryData` en el loader?

> [!question]- Respuesta
> Si el dato ya está en caché (y fresco), lo devuelve sin pedirlo; si no, lo pide y lo guarda. Así cuando el componente monta, el dato **ya está**.

**3.** ¿Por qué en el componente se usa `useSuspenseQuery` y no `useQuery`?

> [!question]- Respuesta
> `useSuspenseQuery` garantiza que `data` **nunca es `undefined`** (no hay estado `isLoading` que manejar), porque el loader ya lo cargó. El tipo queda limpio.

**4.** ¿Por qué `defaultPreloadStaleTime: 0` en `src/router.tsx`?

> [!question]- Respuesta
> Para que el Router no tenga su propia caché paralela y siempre ejecute el loader; quien decide si el dato está viejo es **React Query** (con su `staleTime`).

---

## Paso 5 — TanStack Start (≈ 1–2 h, sin profundizar)

📖 [[TanStack Router y Start]]: secciones 11 a 14.

> [!warning]
> AgriTech migra a **Astro**: esto es lo que menos se reutiliza. Entendelo, no lo domines.

🔍 Leé en AgriTech:
- [ ] `src/routes/__root.tsx` completo (`head`, `shellComponent`, `HeadContent`, `Scripts`)
- [ ] `src/server.ts` (solo por encima: manejo de errores SSR custom)

### ✅ Autoevaluación

**1.** ¿Qué agrega Start que el Router solo no tiene? Nombrá tres cosas.

> [!question]- Respuesta
> **SSR** (HTML renderizado en el servidor), **server functions** (`createServerFn`) y **server routes** (endpoints `/api/...`). Además, deploy a distintos targets (AgriTech: Cloudflare Workers vía Nitro).

**2.** ¿Qué pasa si borrás `<Scripts />` del `RootShell`?

> [!question]- Respuesta
> El HTML se ve (llega del SSR), pero **la app no se hidrata**: ningún botón, dialog ni link de cliente funciona.

**3.** ¿Cómo hacés que la página `/calculadora` tenga su propio `<title>`?

> [!question]- Respuesta
> Agregando `head: () => ({ meta: [{ title: "..." }] })` en su `createFileRoute`. `<HeadContent />` junta los `head` de todas las rutas activas y la más profunda gana.

**4.** ¿Cómo se llama una server function y dónde tiene que estar un secret (API key)?

> [!question]- Respuesta
> `miFn({ data: { ... } })` (siempre envuelto en `data`). El secret va **dentro de `.handler()`**; ahí nunca llega al bundle del cliente.

**5.** ¿Server function o server route? (a) el formulario de leads de AgriTech; (b) un webhook que manda WhatsApp.

> [!question]- Respuesta
> (a) **Server function**: lo llama tu propia app. (b) **Server route**: lo llama un servicio externo y necesita una URL fija.

**6.** ¿Por qué no podés agregar `tailwindcss()` o `tanstackStart()` a `vite.config.ts` en AgriTech?

> [!question]- Respuesta
> Porque `@lovable.dev/vite-tanstack-config` **ya los incluye**. Agregarlos los duplica y rompe la app.

---

## Paso 6 — Repaso (≈ 30 min)

- [ ] Secciones "Errores comunes" y "Checklist" de las **tres** notas.

### ✅ Autoevaluación final (mezcla)

**1.** Describí qué pasa, en orden, desde que hacés clic en `<Link to="/blog/$slug" params={{ slug: "x" }}>` hasta que ves el post.

> [!question]- Respuesta
> El Router matchea la ruta `/blog/$slug` → (si hay) corre `beforeLoad` → corre el `loader` con `params.slug` → si hay `pendingComponent` y tarda, lo muestra → el loader retorna el post (o `notFound()`) → se renderiza `BlogPostPage`, que lee el post con `Route.useLoaderData()`. Bonus: con `defaultPreload: "intent"` en `createRouter`, el loader se **precarga** al pasar el mouse sobre el link (AgriTech no lo tiene activado).

**2.** Te piden "un modal con un formulario que guarda un lead y muestra un toast". Nombrá cada pieza del stack que usarías.

> [!question]- Respuesta
> `Dialog` de shadcn (Radix por debajo, `DialogTrigger asChild`) + `useState` y `zod.safeParse` (patrón del LeadForm, sin RHF) + **server function** POST + `useMutation` (opcional `invalidateQueries`) + `toast` de `sonner`. Colores con tokens de `styles.css`.

**3.** ¿Qué tres cosas **nunca** hacés en AgriTech?

> [!question]- Respuesta
> Editar `routeTree.gen.ts` · agregar plugins a `vite.config.ts` · usar colores hex/paleta de Tailwind en vez de tokens. (También: force-push a la rama conectada a Lovable.)

---

## Paso 7 — Practicar: "Mercado de Cosecha" (1 fin de semana)

📖 [[TanStack Router y Start]]: sección 17.

- [ ] Feature 1 — Lista (server fn + loader + Query)
- [ ] Feature 2 — Filtros en la URL
- [ ] Feature 3 — Detalle + `notFound` + skeleton
- [ ] Feature 4 — Publicar lote (`Dialog` + `useMutation`)
- [ ] Feature 5 — Ofertar (optimistic update)
- [ ] Feature 6 — `/api/lotes`
- [ ] Feature 7 — Bonus: `beforeLoad` + `redirect`

### ✅ Autoevaluación (después de terminar)

**1.** En la feature 5, si la server function falla, ¿cómo vuelve la UI al estado anterior?

> [!question]- Respuesta
> En `onMutate` guardás un snapshot (`queryClient.getQueryData(key)`) y lo retornás como contexto; en `onError` hacés `queryClient.setQueryData(key, context.snapshot)`. En `onSettled`, `invalidateQueries` para sincronizar con el servidor.

**2.** ¿Cómo comprobás que la lista de lotes llega renderizada por SSR?

> [!question]- Respuesta
> "Ver código fuente" (no el inspector): si los nombres de los lotes aparecen en el HTML crudo, vinieron del servidor.

**3.** ¿Algún componente tuyo usa `useEffect` para traer datos? Si sí, ¿cómo lo reemplazás?

> [!question]- Respuesta
> No debería. Se reemplaza por `loader` + `ensureQueryData` (o `useQuery` si es un dato secundario que no bloquea la página).

---

## Paso 8 — Volver a AgriTech

Primer cambio real para fijar todo: **una ruta nueva + un `Dialog` de shadcn**, usando tokens de `styles.css` (nada de hex).

- [ ] Ruta nueva en `src/routes/` que compone `<NavBar />` + sección + `<Footer />`
- [ ] Sección en `src/components/sections/` (carpeta propia si crece, como `calculator/`)
- [ ] `bun run lint` sin errores nuevos

---

## Resumen

| # | Qué | Nota | Secciones | Tiempo |
| --- | --- | --- | --- | --- |
| 1 | shadcn + Radix | [[Shadcn UI]] | 1 a 9 | 2–3 h |
| 2 | React Query | [[React Query (Tanstack query)]] | 1 a 9 | 2–3 h |
| 3 | Router | [[TanStack Router y Start]] | 1 a 9 | 3–4 h |
| 4 | Router + Query | [[TanStack Router y Start]] | 10 | 1 h |
| 5 | Start | [[TanStack Router y Start]] | 11 a 14 | 1–2 h |
| 6 | Repaso | las tres | Errores + Checklist | 30 min |
| 7 | Mini proyecto | [[TanStack Router y Start]] | 17 | fin de semana |
| 8 | Cambio real en AgriTech | — | — | 2–4 h |
