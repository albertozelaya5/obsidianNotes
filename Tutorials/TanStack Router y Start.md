**TanStack Router** es un router para React **100% tipado**: rutas, params y search params se chequean en compilación. **TanStack Start** es el framework full-stack construido encima del Router: le suma SSR, server functions y endpoints de API (como Next.js, pero con el Router de TanStack).

Notas basadas en **AgriTech WebSite** (Banhcafe × IFC): `@tanstack/react-router` v1.170, `@tanstack/react-start` v1.168, `@tanstack/react-query` v5, generado en Lovable. Ver también [[React Query (Tanstack query)]] y [[Shadcn UI]].

## Índice

1. La familia TanStack (quién es quién)
2. **Router**: file-based routing y nombres de archivo
3. **Router**: `createFileRoute` y la anatomía de una ruta
4. **Router**: `<Link>` y `useNavigate`
5. **Router**: params dinámicos (`$slug`)
6. **Router**: search params tipados (`validateSearch`)
7. **Router**: `loader`, `notFound`, pending y error
8. **Router**: layouts con `<Outlet />`
9. **Router**: `beforeLoad` y `redirect` (guards)
10. **Router + Query**: `ensureQueryData` + `useSuspenseQuery`
11. **Start**: qué agrega encima del Router
12. **Start**: `__root.tsx` y `head`
13. **Start**: server functions (`createServerFn`)
14. **Start**: server routes (endpoints de API)
15. Errores comunes
16. Checklist
17. 🧪 Mini proyecto de práctica: **"Mercado de Cosecha"**

---

## 1. La familia TanStack (quién es quién)

| Paquete | Qué hace | ¿Lo usa AgriTech? |
| --- | --- | --- |
| `@tanstack/react-router` | Rutas, navegación, params, loaders | ✅ sí, en todo |
| `@tanstack/react-start` | SSR, server functions, endpoints | ✅ sí (el app shell) |
| `@tanstack/react-query` | Estado de servidor / caché | ✅ instalado, `QueryClient` en el context |
| `@tanstack/react-table` | Tablas headless | ❌ se agregó y se quitó (se prefirió `.map()`) |

> [!tip] Modelo mental
> **Router** = "¿qué página muestro y con qué datos?" · **Start** = "¿qué corre en el servidor?" · **Query** = "¿cómo cacheo los datos del servidor?"

---

## 2. Router: file-based routing y nombres de archivo

Cada archivo en `src/routes/` **es una ruta**. El plugin genera `src/routeTree.gen.ts` automáticamente → **nunca lo edités a mano**.

| Archivo | URL | Notas |
| --- | --- | --- |
| `__root.tsx` | (todas) | App shell. Siempre se renderiza |
| `index.tsx` | `/` | |
| `calculadora.tsx` | `/calculadora` | |
| `blog.$slug.tsx` | `/blog/:slug` | El `.` equivale a `/` |
| `blog/$slug.tsx` | `/blog/:slug` | Igual que arriba, pero con carpeta |
| `blog.tsx` + `blog.index.tsx` | `/blog` | `blog.tsx` se vuelve **layout** de sus hijos |
| `_app.tsx` | (sin URL) | `_` = layout **sin path** (pathless) |
| `{-$lang}.tsx` | `/` o `/:lang` | Param **opcional** |
| `$.tsx` | `/*` | Splat (catch-all) |

En AgriTech: `index`, `calculadora`, `productos`, `proveedores`, `genetica-bovina`, `blog.$slug`.

---

## 3. Router: `createFileRoute` y la anatomía de una ruta

```tsx
import { createFileRoute } from "@tanstack/react-router";

export const Route = createFileRoute("/calculadora")({
  component: CalculatorPage,      // qué renderiza
  // loader, validateSearch, beforeLoad, head, pendingComponent, errorComponent...
});

function CalculatorPage() {
  return <main>...</main>;
}
```

- El string (`"/calculadora"`) lo **escribe y mantiene el plugin**: si renombrás el archivo, se actualiza solo.
- **Tiene que llamarse `Route` y estar exportado.**
- Desde el componente accedés a todo vía `Route.useX()`: `Route.useParams()`, `Route.useSearch()`, `Route.useLoaderData()`.

---

## 4. Router: `<Link>` y `useNavigate`

```tsx
import { Link, useNavigate } from "@tanstack/react-router";

<Link to="/calculadora">Calculadora</Link>

// Con param — TS se queja si falta `params`
<Link to="/blog/$slug" params={{ slug: "cafe-resiliente" }}>Leer</Link>

// Con hash (así lo usa el Navbar de AgriTech)
<Link to="/" hash="blog">Volver al blog</Link>

// Estilo cuando está activo
<Link to="/productos" activeProps={{ className: "text-primary font-bold" }}>
  Productos
</Link>
```

Navegación imperativa (después de un submit, por ejemplo):

```tsx
const navigate = useNavigate();
navigate({ to: "/blog/$slug", params: { slug } });
```

> [!important]
> `to` es **la ruta con el `$`**, no la URL ya armada. ❌ `to={"/blog/" + slug}` → ✅ `to="/blog/$slug" params={{ slug }}`. Así TS te valida todo.

---

## 5. Router: params dinámicos (`$slug`)

Archivo `blog.$slug.tsx` → `/blog/lo-que-sea`.

```tsx
export const Route = createFileRoute("/blog/$slug")({ component: Post });

function Post() {
  const { slug } = Route.useParams(); // tipado: string
}
```

---

## 6. Router: search params tipados (`validateSearch`)

El **query string es estado** (filtros, página, tab). El Router lo valida y lo tipa. Acepta Zod directamente (Standard Schema):

```tsx
import { z } from "zod";

const searchSchema = z.object({
  q: z.string().optional(),
  cultivo: z.enum(["cafe", "cacao", "maiz"]).optional(),
  page: z.number().int().min(1).catch(1),  // .catch = valor si viene basura
});

export const Route = createFileRoute("/proveedores")({
  validateSearch: searchSchema,
  component: Providers,
});

function Providers() {
  const { q, cultivo, page } = Route.useSearch();
  const navigate = Route.useNavigate();

  // Actualizar UN param sin perder los demás
  const setCultivo = (c: string) =>
    navigate({ search: (prev) => ({ ...prev, cultivo: c, page: 1 }) });
}
```

En un `<Link>`:

```tsx
<Link to="/proveedores" search={{ cultivo: "cafe", page: 1 }}>Café</Link>
```

> [!tip] ¿Por qué en la URL y no en `useState`?
> Se puede compartir el link, sobrevive al refresh y el botón "atrás" funciona. Es la "URL state" que menciona el `CLAUDE.md` del proyecto como alternativa a un store global.

---

## 7. Router: `loader`, `notFound`, pending y error

El `loader` corre **antes** de renderizar la ruta (en el servidor con SSR, o en el cliente al navegar). Ejemplo real de `blog.$slug.tsx`:

```tsx
export const Route = createFileRoute("/blog/$slug")({
  component: BlogPostPage,
  loader: ({ params }) => {
    const post = blogPosts.find((p) => p.slug === params.slug);
    if (!post) throw notFound();   // → renderiza el notFoundComponent
    return post;
  },
});

function BlogPostPage() {
  const post = Route.useLoaderData(); // tipado con lo que retorna el loader
}
```

Estados de la ruta:

```tsx
createFileRoute("/x")({
  loader: ...,
  pendingComponent: () => <Spinner />,           // mientras carga
  errorComponent: ({ error, reset }) => <...>,   // si el loader tira error
  notFoundComponent: () => <p>No existe</p>,     // si tira notFound()
});
```

Si el loader depende de search params, declarálo con `loaderDeps` (si no, no se re-ejecuta al cambiar el filtro):

```tsx
loaderDeps: ({ search }) => ({ q: search.q }),
loader: ({ deps }) => fetchProviders(deps.q),
```

> [!note]
> AgriTech define `NotFoundComponent` y `ErrorComponent` globales en `__root.tsx`. Cualquier ruta sin los suyos usa esos.

---

## 8. Router: layouts con `<Outlet />`

Un archivo "padre" renderiza `<Outlet />` donde van los hijos.

```
routes/
  dashboard.tsx          ← layout: sidebar + <Outlet />
  dashboard.index.tsx    ← /dashboard
  dashboard.ajustes.tsx  ← /dashboard/ajustes
  _auth.tsx              ← layout SIN url (ej: chequea login)
  _auth.perfil.tsx       ← /perfil (envuelto por _auth)
```

```tsx
// dashboard.tsx
export const Route = createFileRoute("/dashboard")({
  component: () => (
    <div className="flex">
      <Sidebar />
      <Outlet />
    </div>
  ),
});
```

> [!note]
> AgriTech **no** usa layouts anidados: cada página compone `<NavBar />` + secciones + `<Footer />` a mano. Es válido para una landing.

---

## 9. Router: `beforeLoad` y `redirect` (guards)

Corre **antes** del loader. Ideal para auth o para inyectar cosas al `context`.

```tsx
import { redirect } from "@tanstack/react-router";

export const Route = createFileRoute("/_auth")({
  beforeLoad: ({ context, location }) => {
    if (!context.user) {
      throw redirect({ to: "/login", search: { redirect: location.href } });
    }
  },
});
```

---

## 10. Router + Query: `ensureQueryData` + `useSuspenseQuery`

AgriTech ya deja el `QueryClient` en el **context del router** (`src/router.tsx` + `createRootRouteWithContext<{ queryClient: QueryClient }>()` en `__root.tsx`). El patrón:

```tsx
import { queryOptions, useSuspenseQuery } from "@tanstack/react-query";

// 1. Definí la query UNA vez
const providersQuery = (q?: string) =>
  queryOptions({
    queryKey: ["providers", { q }],
    queryFn: () => getProviders({ data: { q } }), // server fn (sección 13)
  });

// 2. El loader "precalienta" la caché
export const Route = createFileRoute("/proveedores")({
  validateSearch: searchSchema,
  loaderDeps: ({ search }) => ({ q: search.q }),
  loader: ({ context, deps }) =>
    context.queryClient.ensureQueryData(providersQuery(deps.q)),
  component: Providers,
});

// 3. El componente lee de la caché (ya está, no hay loading)
function Providers() {
  const { q } = Route.useSearch();
  const { data } = useSuspenseQuery(providersQuery(q));
}
```

| ¿Quién hace qué? | |
| --- | --- |
| **Router `loader`** | *Cuándo* traer datos (antes de mostrar la página; o al hacer hover sobre un `<Link>` si activás `defaultPreload: "intent"`) |
| **React Query** | *Cachear*, invalidar después de una mutación, refetch en background |

> [!tip]
> `defaultPreloadStaleTime: 0` en `src/router.tsx` es la configuración recomendada cuando usás Query: le deja a Query la decisión de si los datos están viejos.

---

## 11. Start: qué agrega encima del Router

| Router solo (SPA) | Router + **Start** |
| --- | --- |
| Todo corre en el navegador | **SSR**: el HTML llega renderizado (SEO, primera carga rápida) |
| Llamás a una API externa con `fetch` | **Server functions**: funciones TS que corren en el servidor y llamás como una función normal |
| No tenés backend | **Server routes**: endpoints `GET/POST` en el mismo repo |
| — | Deploy a Node, Cloudflare Workers (AgriTech usa Nitro → Workers), Vercel... |

> [!warning] En AgriTech
> Lovable configura todo en `@lovable.dev/vite-tanstack-config`. **No agregués plugins a `vite.config.ts`.** Además el proyecto migra a **Astro**, así que lo de Start es lo que menos se va a reutilizar: aprendé lo suficiente para entenderlo, no más.

---

## 12. Start: `__root.tsx` y `head`

`__root.tsx` arma el `<html>` completo. Piezas clave:

```tsx
export const Route = createRootRouteWithContext<{ queryClient: QueryClient }>()({
  head: () => ({
    meta: [{ title: "AGRITECH BANHCAFE | ..." }, { name: "description", content: "..." }],
    links: [{ rel: "stylesheet", href: appCss }],
  }),
  shellComponent: RootShell,      // <html><head><HeadContent/></head><body>...<Scripts/></body></html>
  component: RootComponent,       // QueryClientProvider + <Outlet />
  notFoundComponent: NotFoundComponent,
  errorComponent: ErrorComponent,
});
```

- `<HeadContent />` → inyecta los `meta`/`links` de **todas** las rutas activas (una ruta hija puede definir su propio `head` y pisar el `title`).
- `<Scripts />` → los scripts de hidratación. Sin esto la app no es interactiva.

---

## 13. Start: server functions (`createServerFn`)

Una función que **solo corre en el servidor**, pero la llamás desde el cliente como si fuera local (Start crea el endpoint y el `fetch` por vos).

```ts
// src/server/providers.ts
import { createServerFn } from "@tanstack/react-start";
import { z } from "zod";

export const getProviders = createServerFn({ method: "GET" })
  .inputValidator(z.object({ q: z.string().optional() }))
  .handler(async ({ data }) => {
    // acá podés usar secrets, DB, process.env... NUNCA llega al bundle del cliente
    return db.providers.filter((p) => !data.q || p.name.includes(data.q));
  });

export const createLead = createServerFn({ method: "POST" })
  .inputValidator(z.object({ name: z.string().min(2), phone: z.string() }))
  .handler(async ({ data }) => {
    await db.leads.insert(data);
    return { ok: true };
  });
```

Uso:

```tsx
// en un loader
loader: () => getProviders({ data: {} }),

// en una mutación de React Query
const mutation = useMutation({
  mutationFn: (input: Lead) => createLead({ data: input }),
  onSuccess: () => queryClient.invalidateQueries({ queryKey: ["leads"] }),
});
```

> [!important]
> El argumento siempre va envuelto: `fn({ data: {...} })`. En versiones viejas era `.validator()`; en la instalada (1.168) es **`.inputValidator()`**.

---

## 14. Start: server routes (endpoints de API)

Para cuando necesitás una URL real (webhooks, que la consuma otra app):

```ts
// src/routes/api/health.ts
import { createFileRoute } from "@tanstack/react-router";

export const Route = createFileRoute("/api/health")({
  server: {
    handlers: {
      GET: async () => Response.json({ ok: true }),
      POST: async ({ request }) => {
        const body = await request.json();
        return Response.json({ received: body });
      },
    },
  },
});
```

> [!tip] ¿Server function o server route?
> ¿Lo llama **tu propia app**? → server function. ¿Lo llama **alguien de afuera**? → server route.

---

## 15. Errores comunes

| Síntoma | Causa | Arreglo |
| --- | --- | --- |
| Error de TS en `<Link to=...>` | Armaste la URL con string | `to="/x/$id" params={{ id }}` |
| La ruta nueva no aparece | El dev server no regeneró `routeTree.gen.ts` | Guardá el archivo de nuevo o reiniciá `bun run dev` |
| Cambio el filtro y el loader no corre | Falta `loaderDeps` | Declarar las deps que usa el loader |
| `Route.useSearch()` devuelve `any` | Falta `validateSearch` | Agregar un schema Zod |
| Editaste `routeTree.gen.ts` y se perdió | Es generado | Nunca tocarlo |
| Secret aparece en el bundle | Lo usaste fuera de un `.handler()` | Solo dentro de server functions |
| Datos duplicados Router/Query | Usás `useLoaderData` **y** `useQuery` | Loader → `ensureQueryData`, componente → `useSuspenseQuery` |

---

## 16. Checklist

- [ ] Sé qué URL genera cada nombre de archivo (`.`, `$`, `_`, `index`, `{-$}`)
- [ ] Uso `<Link to params search>` y nunca concateno URLs
- [ ] Los filtros viven en search params con `validateSearch`
- [ ] Los datos de página se cargan en `loader` (+ `loaderDeps` si dependen del search)
- [ ] Con React Query: `queryOptions` → `ensureQueryData` → `useSuspenseQuery`
- [ ] Sé distinguir server function vs server route
- [ ] Nunca toco `routeTree.gen.ts` ni agrego plugins en `vite.config.ts` (AgriTech)

---

## 17. 🧪 Mini proyecto de práctica: "Mercado de Cosecha"

> Un mini marketplace donde productores publican su cosecha (café, cacao, maíz...) y compradores filtran y hacen ofertas. **~1 fin de semana.** Reutiliza el dominio de AgriTech para que lo aprendido se traslade directo.

### Setup

```bash
bun create @tanstack/start@latest mercado-cosecha
cd mercado-cosecha
bunx shadcn@latest init
bunx shadcn@latest add button card input select dialog badge sonner skeleton
bun add @tanstack/react-query zod
```

"Base de datos": un array en memoria en `src/server/db.ts` (se reinicia con el server; está bien para practicar).

```ts
type Lote = {
  id: string;
  productor: string;
  cultivo: "cafe" | "cacao" | "maiz" | "frijol";
  quintales: number;
  precioPorQuintal: number;
  departamento: string;
  ofertas: { comprador: string; monto: number }[];
};
```

### Rutas

| Archivo | URL | Qué practicás |
| --- | --- | --- |
| `__root.tsx` | — | `head`, `QueryClient` en el context, 404 global, `<Toaster />` |
| `index.tsx` | `/` | `<Link>` con `search` predefinido ("Ver café" → `/lotes?cultivo=cafe`) |
| `lotes.tsx` | — | **Layout**: header con filtros + `<Outlet />` |
| `lotes.index.tsx` | `/lotes?q=&cultivo=&orden=` | `validateSearch`, `loaderDeps`, `ensureQueryData`, `useSuspenseQuery` |
| `lotes.$id.tsx` | `/lotes/:id` | Param, `notFound()`, `pendingComponent`, `head` dinámico |
| `api/lotes.ts` | `/api/lotes` | Server route `GET` que devuelve JSON |

### Features (en orden, cada una suma una habilidad)

1. **Lista de lotes** → `createServerFn` GET + `queryOptions` + `loader` con `ensureQueryData`. UI: grilla de `Card` de shadcn.
2. **Filtros en la URL** → `Input` (búsqueda), `Select` (cultivo) y `Select` (orden: precio ↑/↓). Todo en search params con Zod. *Prueba:* refrescá la página y los filtros se mantienen; copiá la URL en otra pestaña.
3. **Detalle** → `/lotes/$id` con `notFound()` si no existe. Agregá `await new Promise(r => setTimeout(r, 800))` en la server fn para **ver** el `pendingComponent` (un `Skeleton`). `head` con `title: "Café — 40 qq | Mercado de Cosecha"`.
4. **Publicar lote** → `Dialog` de shadcn con un formulario (estilo AgriTech: `useState` + `zod.safeParse`), server fn POST, `useMutation` + `invalidateQueries(["lotes"])`, `toast` de éxito. *Prueba:* el lote nuevo aparece sin recargar.
5. **Ofertar** → en el detalle, botón "Hacer oferta" (`Dialog`). Mutación con **optimistic update** (`onMutate` / `onError` rollback). *Prueba:* hacé que la server fn tire error si `monto < precioPorQuintal * 0.5` y comprobá que la UI vuelve atrás.
6. **Endpoint público** → `GET /api/lotes` devuelve el JSON. Abrilo en el navegador.
7. **Bonus** → layout `_productor.tsx` con `beforeLoad` que hace `redirect` a `/` si no hay `?rol=productor` en la URL (auth de juguete).

### Mapa: habilidad → dónde la practicás

| Habilidad | Feature |
| --- | --- |
| File routing, `$param`, layout + `<Outlet />` | Rutas, 3 |
| `<Link>` tipado, `useNavigate` con `search: prev => ...` | 2 |
| `validateSearch` + `loaderDeps` | 2 |
| `loader`, `notFound`, `pendingComponent`, `head` | 1, 3 |
| `beforeLoad` + `redirect` | 7 |
| `createServerFn` GET/POST + `inputValidator` | 1, 4, 5 |
| Server route | 6 |
| Query: `queryOptions`, `ensureQueryData`, `useSuspenseQuery` | 1 |
| Query: `useMutation`, `invalidateQueries`, optimistic update | 4, 5 |
| shadcn: `Card`, `Select`, `Dialog`, `Skeleton`, `sonner` + Radix `asChild` | 1–5 |
| Tailwind tokens (`bg-primary`, no hex) | todo |

### Criterio de "terminado"

- [ ] `bunx tsc --noEmit` sin errores (el Router te va a obligar a tipar todo)
- [ ] Ningún `useEffect` para traer datos
- [ ] Ninguna URL armada con strings
- [ ] Los filtros sobreviven a F5
- [ ] Ver el HTML con "View Source" → los lotes ya vienen renderizados (SSR ✅)
