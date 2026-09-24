Para arreglarlo:

1. Abre claude1.
2. Escribe /logout.
3. Escribe /login. Cuando se abra el navegador, entra con la otra cuenta. Si te vuelve a meter directo con esta, copia la URL de login en una ventana de incógnito o cierra sesión en claude.ai antes.
4. Para confirmar la cuenta, usa /status dentro de claude1. Desde la terminal también puedes correr:
grep emailAddress ~/.claude-accJen/.claude.json

claude2 (~/.claude-accAl) ya tiene esta cuenta, así que no tienes que tocarlo. Cada carpeta guarda sus credenciales por separado en el Keychain, así que al cambiar claude1 no cambia claude2.

## Contexto para Claude

Exacto — el CLAUDE.md ya está guardado en el repo (/Users/albertozelaya/Documents/banhcafe-projects/BANHCAFE-Pyme/CLAUDE.md), así que no necesitas que yo te lo "pase": simplemente abrí ese archivo y copia su contenido, o directamente decile a la otra sesión de Claude que lea CLAUDE.md en ese mismo repo (si la abrís desde esa carpeta, Claude Code lo carga automáticamente al inicio, sin que tengas que pegarlo a mano).
## Qué es

El proyecto es para ver la facturación de las PYMES, va a usar Typescript, zustand, react query, tailwind

La pantalla principal sera un Dashboard que muestre facturas pendientes, ingresos, resumen de facturas y resumen de ventas

- Vender — pantalla de punto de venta (POS). Buscás productos por nombre/código, los agregás al carrito, seleccionás cliente y procesás el cobro ("Ir al pago"), una vez pagado, se piuede descargar el recibo, enviar por email o imprimir
- Pedidos — lista de pedidos abiertos/pendientes (distinto de una venta directa en "Vender"). Se puede filtrar por artículo/cliente y por estado, esta no la pude ver mucho porque no me permite editarla xd
- Productos — catálogo de artículos del negocio: alta, edición, categorías, exportar e importar productos.
- Catálogo Online — genera una tienda web pública (link tipo tunombre.kyte.site) para que los clientes vean/compren productos online. Pide nombre del comercio e identificación fiscal. - esto no se si es que solo te hago un POST para crear el sitio y ya, o shit si te crea la pagina
- Clientes — CRM básico: registro y búsqueda de clientes para asociarlos a ventas/pedidos.
- Transacciones — historial de ventas con resumen rápido (hoy, ayer, semana, mes) y búsqueda por cliente/producto.
- Finanzas — control de cuentas por pagar (gastos con vencimiento) y registro de salidas de dinero.
- Estadísticas — dashboard de métricas: facturación, cantidad de ventas, ticket medio, ganancia, tasa de venta, medios de pago y productos/clientes top, filtrable por período.
- Usuarios — gestión de usuarios del sistema (owner, empleados) con visibilidad de facturación/ventas por usuario y permisos.
- Configuraciones — ajustes generales del negocio, divididos en pestañas:
    - General: datos de identificación, contacto, dirección, logo, moneda.
    - Pedidos y ventas: tasas/impuestos de venta y estados personalizables del flujo de pedidos.
    - Recibo: personalización del ticket/recibo impreso (datos de tienda, encabezado/pie).
    - Pagos: métodos de pago aceptados (saldo cliente/fiado, link de pago, pagos automáticos, lectores de tarjeta).
    - Entrega y retirada / Integraciones: opciones de logística y conexiones externas (no capturadas en detalle aquí).

En resumen: es un sistema tipo POS + e-commerce simple para PyMEs — vender, gestionar pedidos/productos/clientes, ver finanzas y estadísticas, y configurar el negocio.

Todas las secciones estan sujetas a cambios, por lo que debe ser un proyecto escalable

## Stack

| Área | Herramienta |

| ------------------ | -------------------------------------------------------- |

| UI | React 19 + TypeScript (Vite) |

| Routing | `react-router` v8 (data router) |

| Primitivos de UI | **shadcn/ui** (Radix) en `src/components/ui/` |

| Formularios | **React Hook Form** |

| Validación | **Zod** (`zodResolver` de `@hookform/resolvers`) |

| Estado de servidor | **TanStack Query** (`@tanstack/react-query`) |

| Estado de cliente | `zustand` (sesión + UI cross-cutting) |

| Estilos | Tailwind CSS v4 (`@theme`) |

| Iconos | `lucide-react` |

| Lint | `oxlint` |

| Formato | Prettier + `prettier-plugin-tailwindcss` (ordena clases) |

| Package | `pnpm` |

  

> [!NOTE]

> Alias de imports: **`@/` → `src/`** (configurado en `tsconfig*.json` y `vite.config.ts`). El código

> nuevo importa con `@/...`; quedan imports relativos antiguos que se migran al tocarlos.

  

### shadcn/ui

  

- Config en `components.json` (estilo `new-york`, `baseColor` neutral, CSS vars). El bloque de tokens

shadcn (`:root` oklch, `.dark`, `@theme inline`) ya vive en `src/index.css`, **encima** del `@theme`

del design system Banhcafe — el rojo de marca (`#a41f35`) sobre-escribe a propósito `--color-primary`

de shadcn (primary = acción primaria en ambos sistemas).

- Agregar primitivos: `pnpm dlx shadcn@latest add <componente>`. Aterrizan en `src/components/ui/` con

nombre **en minúsculas** (`button.tsx`, `dialog.tsx`) — es la única excepción a la regla PascalCase.

- Los archivos generados **se editan in-place** (no se envuelven). Si un primitivo necesita variantes de

marca, se ajustan sus `cva()` directamente.

- `cn()` sigue en `src/lib/utils.ts` (`clsx` + `tailwind-merge`); shadcn lo espera en `@/lib/utils`.

  

## Comandos (pnpm)

  

| Comando | Qué hace |

| ------------------------------ | ------------------------------------------ |

| `pnpm dev` | Servidor de desarrollo (Vite + HMR) |

| `pnpm build` | `tsc -b && vite build` (typecheck + build) |

| `pnpm preview` | Sirve el build de producción localmente |

| `pnpm lint` | `oxlint` |

| `pnpm format` | `prettier --write .` (agregar el script) |

| `pnpm exec prettier --write .` | Formatear sin script |

| `pnpm add <pkg>` | Dependencia de runtime |

| `pnpm add -D <pkg>` | Dependencia de desarrollo |

  

Antes de dar una tarea por terminada: `pnpm lint` y `pnpm build` deben pasar, y el código debe quedar formateado con Prettier.

  

### Formato

  

Prettier con **`prettier-plugin-tailwindcss`** (config en `prettier.config.js`) ordena automáticamente las clases de Tailwind en el orden recomendado. Por eso:

  

- No ordenar clases a mano ni pelear con el orden que deja el plugin.

- El plugin también ordena clases dentro de funciones tipo `cn()` / `clsx()`; declararlas con `tailwindFunctions` en `prettier.config.js`.

- Al agregar utilidades custom vía `@utility` o `@theme`, dejar que Prettier reordene al guardar.

  

## Nomenclatura

  

- Carpetas y archivos **en inglés**.

- Componentes React propios: **`PascalCase.tsx`** (`DebtorTable.tsx`).

- **Excepción:** primitivos generados por shadcn en `src/components/ui/` conservan su nombre en

minúsculas (`button.tsx`, `input-otp.tsx`) — no renombrarlos, romperían el `add`/`diff` de shadcn.

- TS no-componente (hooks, stores, utils, types): **`camelCase.ts`** (`useDebounce.ts`, `collectionsStore.ts`).

- Un componente = una carpeta **cuando** tiene tipos/estilos/tests/subcomponentes propios; si es un archivo suelto, sin carpeta.

- Cada carpeta con API pública expone un **`index.ts`** barrel; el resto del código importa del barrel, nunca de rutas internas.

  

## Estructura de carpetas

  

**Recomendación: _feature-first_ con una capa `ui/` compartida.** No es "pages vs features": se usan **ambas** con roles distintos.

  

```

src/

├── app/ # shell: providers, router, layout raíz, error boundary

│ ├── App.tsx

│ ├── router.tsx

│ └── providers.tsx

├── pages/ # 1 archivo por ruta. Capa fina: componen features, sin lógica de negocio

│ ├── DashboardPage.tsx

│ ├── CollectionsPage.tsx

│ └── LoginPage.tsx

├── features/ # capacidades de negocio, cada una autocontenida y portable

│ ├── auth/

│ │ ├── components/

│ │ ├── hooks/

│ │ ├── api/

│ │ ├── store/ # slice/store de zustand de la feature

│ │ ├── types.ts

│ │ └── index.ts # API pública — lo ÚNICO que otras carpetas importan

│ └── collections/

│ ├── components/

│ ├── hooks/

│ ├── api/

│ ├── store/

│ ├── types.ts

│ └── index.ts

├── components/

│ └── ui/ # primitivos reutilizables SIN lógica de negocio (design system)

│ ├── Button/

│ ├── Table/

│ ├── Input/

│ └── Modal/

├── hooks/ # hooks realmente globales (useMediaQuery, useDebounce)

├── lib/ # helpers agnósticos: cliente API, cn(), formatters, config

├── stores/ # store global de zustand compuesto por slices (solo estado cross-cutting)

│ ├── index.ts

│ └── slices/

│ └── uiSlice.ts

├── types/ # tipos compartidos entre features

├── index.css # @theme + tokens (el design system de Banhcafe)

└── main.tsx

```

  

### Reglas de la estructura

  

1. **Agrupar por dominio, no por tipo de archivo.** La alternativa "type-first" (`components/`, `containers/`, `hooks/` en la raíz) obliga a tocar 4–5 carpetas por cada cambio; feature-first coloca junto todo lo que un cambio toca.

2. **`pages/` = destinos de ruta.** Delgadas: leen params, componen features y layout. Cero lógica de negocio, cero fetch directo.

3. **`features/` = el negocio.** Cada feature es autocontenida: sus componentes, hooks, llamadas API, store y tipos viven adentro.

4. **Frontera por barrel.** Se importa `features/collections`, nunca `features/collections/components/DebtorTable`. Si el import cruza hacia un archivo interno de otra feature, está mal.

5. **Una feature no importa los internos de otra.** Si dos features comparten algo, ese algo **baja** a `components/ui/`, `hooks/` o `lib/`. La composición entre features ocurre en `pages/`.

6. **`components/ui/` es agnóstico de negocio.** `Button`, `Table`, `Input`, `Modal`, `Badge`, `Card`. Si un componente sabe qué es un "deudor", no va aquí — va en la feature.

7. **Profundidad máx. ~3 niveles** dentro de una feature. Si crece más, es señal de que hay una sub-feature.

  

### Cuándo escalar o simplificar

  

- **App chica / pocas rutas:** empezá con `pages/` + `components/ui/` planos. Creá `features/<x>/` cuando la carpeta de una ruta pase de ~6–7 archivos **o** su lógica se reuse en otra ruta.

- **Este proyecto (panel que va a crecer):** feature-first desde el inicio.

- **Si querés un estándar formal:** Feature-Sliced Design (capas `app → pages → widgets → features → entities → shared`). Suele ser demasiado peso para un panel interno; el híbrido de arriba rinde más.

  

## Componentes reutilizables (`components/ui/`)

  

Los primitivos vienen de **shadcn/ui**; no se escriben a mano. Cada uno es un archivo suelto en

minúsculas (`button.tsx`, `dialog.tsx`, `input.tsx`, `form.tsx`, `input-otp.tsx`, `sonner.tsx`,

`checkbox.tsx`, `label.tsx`, `spinner.tsx`, …), sin carpeta ni `.types.ts` ni barrel.

  

Reglas:

  

- **Agregar:** `pnpm dlx shadcn@latest add table chart …`. Instala deps de Radix solas.

- **Editar in-place.** Las variantes viven en el `cva()` del propio archivo (`variant`, `size`). Ajustar

ahí si hace falta una variante de marca; no crear un wrapper solo para cambiar clases.

- **Sin lógica de negocio ni fetch.** Si un componente sabe qué es un "deudor", va en la feature, no aquí.

- **`Spinner`** (`spinner.tsx`) es propio: `Loader2Icon` con `animate-spin`. Se usa para estados

`isPending` de mutaciones dentro de botones.

- Combinar clases con `cn()` de `@/lib/utils` y dejar que el consumidor sobre-escriba vía `className`.

  

### Charts

  

`pnpm dlx shadcn@latest add chart` trae el wrapper `ChartContainer` sobre **Recharts**. Los colores de

serie salen de los tokens `--chart-1..5` de `src/index.css`.

  

### Envolturas de negocio

  

Un componente que compone primitivos + datos de una feature (p. ej. `DebtorTable` sobre `<Table>`) es

**PascalCase**, vive en `features/<x>/components/` y puede tener su carpeta con tipos/subcomponentes.

  

## Estado con zustand

  

Dos usos, no mezclar:

  

- **Store por feature** (lo normal): estado de esa capacidad. Vive en `features/<x>/store/`.

- **Store global con slices**: solo estado cross-cutting de UI (sidebar, tema, toasts). Vive en `stores/`.

  

```ts

// src/features/collections/store/collectionsStore.ts

import { create } from "zustand";

import type { CollectionFilters } from "../types";

  

interface CollectionsState {

selectedIds: string[];

filters: CollectionFilters;

setFilters: (patch: Partial<CollectionFilters>) => void;

toggleSelected: (id: string) => void;

}

  

export const useCollectionsStore = create<CollectionsState>((set) => ({

selectedIds: [],

filters: { status: "all", search: "" },

setFilters: (patch) => set((s) => ({ filters: { ...s.filters, ...patch } })),

toggleSelected: (id) =>

set((s) => ({

selectedIds: s.selectedIds.includes(id)

? s.selectedIds.filter((x) => x !== id)

: [...s.selectedIds, id],

})),

}));

```

  

Slices para el store global:

  

```ts

// src/stores/slices/uiSlice.ts

import type { StateCreator } from "zustand";

  

export interface UiSlice {

sidebarOpen: boolean;

toggleSidebar: () => void;

}

  

export const createUiSlice: StateCreator<UiSlice> = (set) => ({

sidebarOpen: true,

toggleSidebar: () => set((s) => ({ sidebarOpen: !s.sidebarOpen })),

});

```

  

```ts

// src/stores/index.ts

import { create } from "zustand";

import { createUiSlice, type UiSlice } from "./slices/uiSlice";

  

export const useAppStore = create<UiSlice>()((...a) => ({

...createUiSlice(...a),

}));

```

  

Reglas:

  

- Seleccionar **campos puntuales**, no el store entero: `useCollectionsStore((s) => s.filters)`.

- **El estado de servidor NO vive en zustand.** Va en TanStack Query (abajo). zustand es solo estado de

cliente: sesión (`features/auth/store/authStore.ts`) y UI cross-cutting (sidebar, tema).

- Un slice por dominio de estado; se agregan al `create` con spread.

  

## Datos, formularios y validación

  

### Peticiones — TanStack Query

  

- `QueryClient` se crea en `src/app/providers.tsx` (`AppProviders`), que envuelve al `RouterProvider` en

`main.tsx`. Ahí también vive el `<Toaster>` de `sonner`.

- Las funciones de red viven en `features/<x>/api/` y usan `apiFetch` de **`src/lib/apiClient.ts`**

(wrapper de `fetch`: base `VITE_BASE_API_URL`, `x-api-key` si está en env, `Authorization: Bearer`

cuando `withAuth`, y `ApiError { status, data }` en fallo — `data` es el body ya parseado).

- Los componentes consumen esas funciones con `useQuery` / `useMutation`, nunca con `fetch` directo.

- `queryKey` con prefijo de feature: `["collections", "list", filters]`.

- Efectos post-mutación (`onSuccess`) invalidan con `queryClient.invalidateQueries`.

- `loader`/`action` de react-router quedan para redirecciones de sesión; el data fetching de pantalla es

Query.

  

### Formularios — React Hook Form + Zod

  

- Un esquema Zod por formulario en `features/<x>/schemas/`; el tipo sale de `z.infer`.

- `useForm({ resolver: zodResolver(schema) })`. Envolver con los primitivos `Form*` de shadcn

(`<Form>`, `<FormField>`, `<FormItem>`, `<FormControl>`, `<FormMessage>`), que cablean `id`,

`aria-invalid` y el mensaje de error desde `fieldState`.

- Campos no triviales (OTP, selects, checkbox) van con `<Controller>` / el `render` de `<FormField>`,

no con `register` + `setValue` manual.

- El `onSubmit` llama a la mutación; los errores de servidor se muestran con `form.setError` (inline) o

`toast.error` (globales).

  

## Routing

  

Data router en `src/app/router.tsx`, páginas con `lazy`:

  

```tsx

import { createBrowserRouter } from "react-router";

  

export const router = createBrowserRouter([

{

path: "/login",

lazy: async () => ({

Component: (await import("../pages/LoginPage")).default,

}),

},

{

path: "/",

lazy: async () => ({ Component: (await import("./AppLayout")).default }),

children: [

{

index: true,

lazy: async () => ({

Component: (await import("../pages/DashboardPage")).default,

}),

},

{

path: "collections",

lazy: async () => ({

Component: (await import("../pages/CollectionsPage")).default,

}),

},

],

},

]);

```

  

- Rutas protegidas: guard en el layout (`AppLayout`) que redirige a `/login` si no hay sesión.

- `loader`/`action` de react-router para carga y mutaciones ligadas a la ruta; lógica pesada en `features/<x>/api/`.

  

## Filosofía visual (accionable)

  

Mezcla: base **Plain/Neutral** + superficie **Startup/Upbeat** + acentos **Calm/Peaceful**. En concreto:

  

- **Layout:** estructurado y denso. Tablas y formularios que se leen claro. Filas de cards para resúmenes/KPIs.

- **Tipografía:** una sola sans-serif (`--font-sans`, Open Sans). Body medio (`text-md` = 1.6rem / 16px, `line-height` 1.5). Texto secundario en color más claro (`text-text-subtitle`, `--color-light`).

- **Bordes y sombras:** `rounded-lg` en cards/inputs/botones. Sombras sutiles: `shadow-card`, `shadow-container`, `shadow-submenu`. Nada de glows.

- **Color:** fondos claros (`bg-background`, `bg-body`, `bg-gray-background`). `primary` `#a41f35` **reservado** para acciones primarias y highlights clave — no decorar con él.

- **Estados:** usar el set de tokens `success` / `warning` / `error` / `info` (`-light` para fondo, `-main` para texto/borde).

- **Iconos:** sí, frecuentes, simples.

- **Sin** ilustraciones 3D, gradientes llamativos ni Z-patterns de landing: es una herramienta interna.

  

## Design tokens (fuente de verdad)

  

Todo vive en **`src/index.css`** (importado desde `src/main.tsx`). Orden actual del archivo:

  

1. `@import "tailwindcss"` + `@import "tw-animate-css"` + `@import "shadcn/tailwind.css"`.

2. Capa **shadcn**: `@theme inline` (mapea `--color-*` → `var(--*)`), `:root` con tokens oklch, `.dark`.

3. Capa **design system Banhcafe**: el `@theme { … }` de abajo (spacing, text, font-weight, colores de

marca, sombras) — se declara **después**, así que gana en los tokens que redefine (p. ej.

`--color-primary: #a41f35`). No sobre-escribir `--color-input` aquí (rompe el borde de los `<Input>`).

  

Notas de escala: `html { font-size: 62.5% }` ⇒ `1rem = 10px`. La escala de spacing arranca en `--spacing-1: 0.4rem` (4px), así que `p-2` = 8px, `p-4` = 16px.

  

El bloque `@theme` de referencia (design system Banhcafe):

  

```css

@import "tailwindcss";

@import "tailwind-animations";

@plugin "@tailwindcss/typography";

  

/* ── Tema ── */

@theme {

--font-sans: var(--font-open-sans), system-ui, sans-serif;

  

/* Spacing — reemplaza los defaults (igual que theme.spacing sin extend) */

--spacing-*: initial;

--spacing-0: 0;

--spacing-1: 0.4rem;

--spacing-2: 0.8rem;

--spacing-3: 1.2rem;

--spacing-4: 1.6rem;

--spacing-5: 2rem;

--spacing-6: 2.4rem;

--spacing-7: 2.8rem;

--spacing-8: 3.2rem;

--spacing-9: 3.6rem;

--spacing-10: 4rem;

--spacing-11: 4.4rem;

--spacing-12: 4.8rem;

--spacing-13: 5.2rem;

--spacing-14: 5.6rem;

--spacing-15: 6rem;

--spacing-16: 6.4rem;

--spacing-header: var(--header-height);

  

/* Font size — reemplaza defaults.

Escala guía (px): 10 12 14 16 18 20 24 30 36 44 52 62 74 86 98.

line-height: 1.5 en texto de lectura, < 1.5 y bajando en texto grande.

letter-spacing: 0 a -0.1rem (-1px), tracking negativo solo en tamaños grandes. */

--text-*: initial;

--text-xs: 1.2rem;

--text-xs--line-height: 1.5;

--text-sm: 1.4rem;

--text-sm--line-height: 1.5;

--text-md: 1.6rem;

--text-md--line-height: 1.5;

--text-lg: 1.8rem;

--text-lg--line-height: 1.5;

--text-xl: 2.4rem;

--text-xl--line-height: 1.4;

--text-xl--letter-spacing: -0.025rem;

--text-xxl: 3rem;

--text-xxl--line-height: 1.3;

--text-xxl--letter-spacing: -0.03rem;

--text-h4: 3.6rem;

--text-h4--line-height: 1.2;

--text-h4--letter-spacing: -0.05rem;

--text-h3: 4.4rem;

--text-h3--line-height: 1.15;

--text-h3--letter-spacing: -0.05rem;

--text-h2: 5.2rem;

--text-h2--line-height: 1.1;

--text-h2--letter-spacing: -0.1rem;

--text-5xl: 6.2rem;

--text-5xl--line-height: 1.1;

--text-5xl--letter-spacing: -0.1rem;

--text-h1: 7.4rem;

--text-h1--line-height: 1.05;

--text-h1--letter-spacing: -0.1rem;

  

/* Font weight — reemplaza defaults */

--font-weight-*: initial;

--font-weight-thin: 350;

--font-weight-normal: 450;

--font-weight-semibold: 550;

--font-weight-bold: 620;

--font-weight-black: 690;

  

/* Colores — extiende defaults (extend.colors en v3) */

--color-primary: #a41f35;

--color-primary-hover: rgba(186, 12, 47, 1);

--color-secondary: #f8f8f8;

--color-tertiary: rgba(52, 54, 66, 1);

--color-background: #fbfbfb;

--color-gray-background: #f5f5f5;

--color-button: rgba(41, 45, 50, 0.7);

--color-notification-container: rgba(217, 236, 254, 1);

--color-accents-blue: #418fd8;

  

--color-error-light: #fff2f2;

--color-error-main: rgba(186, 12, 47, 1);

  

--color-success-light: rgba(222, 237, 227, 1);

--color-success-main: rgba(38, 183, 105, 1);

  

--color-warning-light: rgba(255, 238, 225, 1);

--color-warning-main: rgba(214, 124, 59, 1);

  

--color-info-light: rgba(217, 236, 254, 1);

--color-info-main: rgba(65, 143, 216, 1);

  

--color-body: #ecedf1;

--color-modal: #f3f5fa;

--color-light-gray: #eeeeee;

--color-light: rgba(0, 0, 0, 0.5);

--color-extra-light: rgba(0, 0, 0, 0.03);

--color-submenu: rgba(243, 245, 250, 0.9);

--color-icon-light: rgba(177, 185, 216, 1);

--color-input: rgba(240, 245, 250, 1);

--color-gray-body: rgba(236, 237, 241, 1);

--color-gray-light: rgba(238, 238, 238, 1);

--color-text-subtitle: rgba(52, 54, 66, 1);

--color-icon-background: rgba(233, 200, 200, 0.18);

--color-icon-background-hover: rgba(233, 200, 200, 0.3);

--color-icon-border: rgba(186, 12, 47, 0.3);

  

/* Background images */

--background-image-promotion-background-top: url("/src/assets/images/promotion-background-top.webp");

--background-image-promotion-background-bottom: url("/src/assets/images/promotion-background-bottom.png");

--background-image-about-us-background: url("/src/assets/images/about-us.webp");

--background-image-container:

linear-gradient(0deg, #ffffff, #ffffff),

radial-gradient(116.16% 288.31% at 93.33% 50%, #c3002f 0%, #ffffff 62.5%);

--background-image-input-box: linear-gradient(0deg, #f0f5fa, #f0f5fa);

  

/* Shadows */

--shadow-container:

0px 12px 30px -50px rgba(53, 62, 74, 0.1),

0px 4px 4px 0px rgba(53, 62, 74, 0.1);

--shadow-card: 0px 80px 28px -60px rgba(84, 0, 14, 0.5);

--shadow-submenu: 0px 4px 10px rgba(101, 111, 126, 0.2);

--shadow-card-shadow: 0px 10px 16px -3px rgba(101, 111, 126, 0.2);

--shadow-quick-link: 0 3px 0 0 rgba(0, 0, 0, 0.1);

--shadow-search-box: 0 0 0 5px rgba(255, 255, 255, 0.2);

--shadow-focus: 0px 0px 0px 4px rgba(164, 31, 53, 0.08);

--shadow-error-shadow: 0px 0px 0px 4px rgba(244, 221, 221, 1);

  

/* Animations */

--animate-progress: progress-bar 20s linear;

  

@keyframes progress-bar {

from {

width: 0%;

}

to {

width: 100%;

}

}

}

  

/* ── Base ── */

@layer base {

html {

@apply bg-white text-zinc-800;

font-size: 62.5%;

}

  

body {

font-size: var(--text-md);

font-family: var(--font-sans);

}

  

:root {

--header-height: 6rem;

}

}

  

/* ── Components ── */

@layer components {

.glass {

box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.15);

border-color: rgba(255, 255, 255, 0.3);

border-width: 1px;

backdrop-filter: brightness(100%) blur(7px);

}

  

.embed-html ol,

.embed-html ul {

padding-left: 2rem;

padding-block: 1rem;

}

.embed-html ol {

list-style-type: decimal;

}

.embed-html ul {

list-style-type: disc;

}

  

.background-texture {

@apply bg-white/70 bg-[url('/src/assets/images/background.webp')] bg-cover bg-center bg-no-repeat bg-blend-overlay;

}

  

.background-texture-primary {

@apply bg-primary/50 bg-[url('/src/assets/images/background.webp')] bg-cover bg-center bg-no-repeat bg-blend-multiply;

}

  

.no-scroll {

overflow: hidden;

@apply pr-4;

}

  

.container {

@apply bg-body/70 shadow-container backdrop-blur;

}

  

.slider {

@apply h-2 w-full rounded-full bg-neutral-100;

}

.slider-thumb {

@apply bg-accents-blue h-4 w-4 -translate-y-1/4 cursor-pointer rounded-lg outline-none;

}

.slider-thumb.slider-thumb-1 {

@apply border-accents-blue border-2 bg-white;

}

.slider-track {

@apply h-2;

}

.slider-track.slider-track-1 {

@apply bg-accents-blue;

}

  

@keyframes slide {

0% {

opacity: 0;

}

8% {

opacity: 1;

}

20%,

30% {

opacity: 1;

}

50% {

opacity: 0;

}

}

  

@keyframes slide-info {

0% {

opacity: 0;

}

8% {

opacity: 1;

}

20%,

30% {

opacity: 1;

}

35% {

opacity: 0;

}

}

  

@keyframes loading-bar {

from {

width: 0;

}

to {

width: 100%;

}

}

}

```

  

## Skills locales

  

En `.agents/skills/` hay skills que aplican a este stack — consultarlas cuando toque:

	  

- `tailwind-css-patterns` — patrones de layout, componentes, accesibilidad, animaciones.

- `frontend-design` — criterios de diseño de interfaz.

- `oxlint`, `vite`, `typescript-advanced-types`.

  

---

  

<details>

<summary>Referencia: definiciones completas de personalidad visual</summary>

  

### `Startup/Upbeat`: software startups y empresas de aspecto moderno

  

Sans-serif medianas, textos y fondos gris claro, elementos redondeados. Usa shadows y border-radius.

  

- **Typography** — Headings medianos (no enormes), normalmente una sola sans-serif en todo el diseño. Tendencia a colores de texto más claros.

- **Colors** — Azules, verdes y morados. Muchos fondos claros (sobre todo gris); gradientes comunes.

- **Images** — Siempre hay imágenes o ilustraciones. Las 3D son modernas. A veces patrones y formas para detalle visual.

- **Icons** — ✅ Muy frecuentes.

- **Shadows** — ✅ Sombras sutiles frecuentes. Los glows se están volviendo modernos.

- **Border-radius** — ✅ Bastante común.

- **Layout** — Filas de cards, filas de features de producto y patrones en Z; también animaciones.

  

### `Calm/Peaceful`: salud y productos centrados en el bienestar del consumidor

  

Para productos y servicios que "cuidan": colores pastel calmados, headings serif suaves e imágenes/ilustraciones acordes.

  

- **Typography** — Serif suaves frecuentes en headings, aunque también sans-serif (p. ej. en software).

- **Colors** — Pastel / lavados: naranjas, amarillos, marrones, verdes, azules claros.

- **Images** — Imágenes e ilustraciones habituales (mucha gente), en armonía con la paleta calmada.

- **Icons** — Bastante frecuentes.

- **Shadows** — Normalmente sin sombras; si acaso, con moderación.

- **Border-radius** — ✅ Algo de border-radius es habitual.

- **Layout** — Todo tipo de layouts, sin tendencia particular.

  

### `Plain/Neutral`: corporaciones consolidadas que no buscan impacto por diseño

  

Diseño que "se quita del medio": tipografías neutras y pequeñas, layout muy estructurado. Común en grandes corporaciones.

  

- **Typography** — Sans-serif cuadradas, body pequeño. Un accent podría ser una tipografía distinta (serif).

- **Colors** — Tipografías neutras; texto normalmente pequeño y sin impacto visual.

- **Images** — Frecuentes pero en formato pequeño; tal vez solo una imagen grande en el header.

- **Icons** — Normalmente sin iconos; si acaso, iconos negros simples y pequeños.

- **Shadows** — Normalmente sin sombras. ❌

- **Border-radius** — Normalmente sin border-radius. ❌

- **Layout** — Estructurado y condensado, con muchas cajas y filas.

  

</details>