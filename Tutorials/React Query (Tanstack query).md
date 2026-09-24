React Query (TanStack Query) administra el **estado de servidor**: los datos que viven en tu API, no en tu app. Se encarga de traerlos, cachearlos, saber cuándo están viejos, re-pedirlos, y reflejar `loading` / `error` sin escribir un solo `useEffect` + `useState`.

Notas basadas en **BANHCAFE Collections**: `@tanstack/react-query` v5, cliente HTTP `apiFetch` (`src/lib/apiClient.ts`), features autocontenidas (`features/<x>/api/`, `features/<x>/hooks/`). Ver también [[Estados RTK Query]] (el equivalente en Redux Toolkit) y [[Global State]].

## Índice

1. El problema que resuelve
2. Modelo mental: server state vs client state
3. Setup en el proyecto
4. `useQuery`: la anatomía
5. Query keys
6. `staleTime` y `gcTime`: el corazón de la caché
7. Refetch: cuándo se vuelve a pedir
8. `useMutation`: escribir datos
9. `invalidateQueries` y actualizar la caché
10. Queries dependientes y condicionales
11. Paginación y listas con filtros
12. El patrón del proyecto: capa `api/` + hook
13. DevTools
14. Errores comunes
15. Checklist

---

## 1. El problema que resuelve

Sin React Query, pedir datos se ve así:

```tsx
function DebtorList() {
  const [data, setData] = useState<Debtor[]>();
  const [error, setError] = useState<Error>();
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    let cancelled = false;
    setLoading(true);
    fetch("/api/debtors")
      .then((r) => r.json())
      .then((d) => !cancelled && setData(d))
      .catch((e) => !cancelled && setError(e))
      .finally(() => !cancelled && setLoading(false));
    return () => {
      cancelled = true;
    };
  }, []);

  // ...y todavía te falta: re-pedir al volver a la pestaña, no re-pedir
  // si ya lo tenés fresco, compartir estos datos con otro componente,
  // reintentar en caso de fallo, invalidar cuando algo cambia...
}
```

Cada pantalla reescribe lo mismo, y ninguna comparte caché con la otra. React Query reemplaza **todo ese bloque** por:

```tsx
const { data, error, isPending } = useQuery({
  queryKey: ["debtors", "list"],
  queryFn: () => apiFetch<Debtor[]>("/debtors"),
});
```

Y regala gratis: caché compartida entre componentes, deduplicación de peticiones, re-fetch inteligente, reintentos, estados de carga/error normalizados y devtools.

---

## 2. Modelo mental: server state vs client state

|              | **Client state**                             | **Server state**                                          |
| ------------ | -------------------------------------------- | -------------------------------------------------------- |
| Ejemplo      | sidebar abierto, filtros del form, tema      | lista de deudores, detalle de un pago                    |
| Dueño        | tu app                                       | el backend (tu copia siempre puede estar vieja)         |
| Herramienta  | `useState` / **zustand**                     | **React Query**                                          |

> [!IMPORTANT] Regla del proyecto
> El estado de servidor **NO** va en zustand. zustand es solo sesión (`authStore`) y UI cross-cutting (sidebar, tema). Todo lo que venga de la API vive en React Query.

React Query no es un cliente HTTP. **Vos** traés los datos (con `apiFetch`); React Query los **administra** (caché, frescura, sincronización).

---

## 3. Setup en el proyecto

Ya está hecho, pero conviene entenderlo. En `src/app/providers.tsx`:

```tsx
const [queryClient] = useState(
  () =>
    new QueryClient({
      defaultOptions: {
        queries: {
          retry: 1,                      // 1 reintento antes de marcar error
          refetchOnWindowFocus: false,   // no re-pedir al volver a la pestaña
          staleTime: 30_000,             // los datos se consideran frescos 30s
        },
      },
    }),
);

return (
  <QueryClientProvider client={queryClient}>
    {children}
    <Toaster richColors position="top-center" />
  </QueryClientProvider>
);
```

Puntos finos:

- **`useState(() => new QueryClient())`**: se crea **una sola vez**. Si hicieras `new QueryClient()` suelto en el cuerpo del componente, cada render crearía uno nuevo y tirarías la caché.
- **`defaultOptions.queries`**: valores por defecto para **todas** las queries. Cada `useQuery` puede sobre-escribirlos.
- `QueryClientProvider` envuelve al router, así que cualquier página puede usar hooks de Query.

---

## 4. `useQuery`: la anatomía

```tsx
import { useQuery } from "@tanstack/react-query";
import { apiFetch } from "@/lib/apiClient";
import type { Debtor } from "../types";

function DebtorList() {
  const query = useQuery({
    queryKey: ["debtors", "list"],
    queryFn: () => apiFetch<Debtor[]>("/debtors"),
  });

  if (query.isPending) return <Spinner />;
  if (query.isError) return <p>Error: {query.error.message}</p>;

  return (
    <ul>
      {query.data.map((d) => (
        <li key={d.id}>{d.name}</li>
      ))}
    </ul>
  );
}
```

### 4.1 Las dos piezas obligatorias

| Opción     | Qué es                                                                                                                        |
| ---------- | --------------------------------------------------------------------------------------------------------------------------- |
| `queryKey` | Identidad **única** de estos datos en la caché. Un array serializable. Ver sección 5.                                        |
| `queryFn`  | Función que devuelve una **promesa** con los datos. Debe **lanzar** (throw) si falla. `apiFetch` ya lanza `ApiError`.        |

### 4.2 Los estados que devuelve

`status` (¿tengo datos?) tiene tres valores:

| `status`      | Significado                             | Booleano     |
| ------------- | -------------------------------------- | ------------ |
| `"pending"`   | Todavía no hay datos en caché          | `isPending`  |
| `"error"`     | La `queryFn` falló y no hay datos      | `isError`    |
| `"success"`   | Hay datos disponibles en `data`        | `isSuccess`  |

`fetchStatus` (¿hay una petición en curso ahora mismo?) es **independiente**:

| `fetchStatus`  | Significado                        | Booleano      |
| -------------- | -------------------------------- | ------------- |
| `"fetching"`   | La `queryFn` se está ejecutando  | `isFetching`  |
| `"idle"`       | No hay petición activa            |               |
| `"paused"`     | Sin conexión, esperando red       |               |

> [!TIP] Por qué son dos ejes separados
> Cuando hacés un **refetch** de datos que ya tenías, `status` sigue siendo `"success"` (seguís mostrando la lista vieja) pero `fetchStatus` pasa a `"fetching"`. Eso te deja mostrar la tabla + un spinnercito arriba, en vez de vaciar la pantalla.
>
> - **Primera carga** → `isPending` (aún no hay nada que mostrar)
> - **Recarga en segundo plano** → `isFetching` (mostrá lo viejo + indicador sutil)

### 4.3 Otras props útiles del resultado

```tsx
const {
  data,          // los datos (undefined mientras isPending)
  error,         // el error tipado (ApiError en este proyecto)
  isPending,     // primera carga, sin datos
  isFetching,    // hay una petición en vuelo (primera o refetch)
  isRefetching,  // isFetching && !isPending
  isSuccess,
  isError,
  refetch,       // () => void  para re-pedir a mano
  isStale,       // ¿los datos ya se consideran viejos?
  dataUpdatedAt, // timestamp de la última vez que data cambió
} = useQuery({ ... });
```

### 4.4 Tipado

El genérico va en `apiFetch`, y React Query **infiere** el resto:

```tsx
const query = useQuery({
  queryKey: ["debtors", "detail", id],
  queryFn: () => apiFetch<Debtor>(`/debtors/${id}`),
});
// query.data  ->  Debtor | undefined
// query.error ->  Error | null   (casteá a ApiError si necesitás error.data)
```

---

## 5. Query keys

La `queryKey` es la **dirección** de esos datos en la caché. React Query:

- Guarda el resultado bajo esa key.
- Si dos componentes usan la **misma** key, comparten la misma entrada de caché (y una sola petición).
- Cuando cualquier valor dentro del array **cambia**, trata eso como *otra* query y vuelve a pedir.

### 5.1 Convención del proyecto

> [!IMPORTANT]
> `queryKey` **siempre con prefijo de feature**, de lo general a lo específico: `["collections", "list", filters]`, `["debtors", "detail", id]`.

```tsx
["debtors"]                       // todo lo de deudores
["debtors", "list"]               // la lista
["debtors", "list", { status }]   // la lista filtrada
["debtors", "detail", id]         // un deudor
```

Este orden jerárquico es lo que hace que la invalidación por prefijo funcione (sección 9).

### 5.2 Reglas

- **Todo lo que la `queryFn` use para pedir, va en la key.** Si tu `queryFn` lee `id` y `filters`, ambos van en la key. Si no, cambiás el filtro y React Query te devuelve la caché vieja.
- Los objetos se comparan por **contenido**, no por referencia (hash estable). `{ a: 1, b: 2 }` y `{ b: 2, a: 1 }` son la misma key. Podés pasar `filters` directo.
- Deben ser serializables: strings, números, booleanos, arrays, objetos planos. Nada de funciones, `Date`, `Map`, instancias de clase.

### 5.3 Fábrica de keys (recomendado cuando la feature crece)

```ts
// src/features/debtors/api/queryKeys.ts
export const debtorKeys = {
  all: ["debtors"] as const,
  lists: () => [...debtorKeys.all, "list"] as const,
  list: (filters: DebtorFilters) => [...debtorKeys.lists(), filters] as const,
  details: () => [...debtorKeys.all, "detail"] as const,
  detail: (id: string) => [...debtorKeys.details(), id] as const,
};

// uso
useQuery({ queryKey: debtorKeys.list(filters), queryFn: ... });
queryClient.invalidateQueries({ queryKey: debtorKeys.lists() }); // invalida TODAS las listas
```

---

## 6. `staleTime` y `gcTime`: el corazón de la caché

Los dos parámetros que más confunden y los más importantes.

### 6.1 `staleTime` — cuánto tiempo los datos se consideran **frescos**

- Mientras están **fresh**: React Query los sirve desde caché y **no** vuelve a pedir, ni al montar otro componente, ni al reenfocar la ventana.
- Cuando pasa el `staleTime` se vuelven **stale** (viejos): siguen visibles, pero el próximo "disparador" (montar, reenfocar, reconectar) provoca un refetch en segundo plano.
- Default global del proyecto: `30_000` (30 s). Default de fábrica de React Query: `0` (todo nace stale).

```tsx
useQuery({
  queryKey: ["collections", "kpis"],
  queryFn: () => apiFetch<Kpis>("/collections/kpis"),
  staleTime: 5 * 60_000, // estos KPIs cambian poco: frescos por 5 min
});
```

Regla de dedo:

| Tipo de dato                          | `staleTime` sugerido           |
| ------------------------------------- | ------------------------------ |
| Cambia constantemente (feed en vivo)  | `0`                            |
| Datos de pantalla normales            | `30s`–`60s` (default proyecto) |
| Catálogos, KPIs, config               | `5min`+                        |
| Prácticamente inmutable               | `Infinity`                     |

### 6.2 `gcTime` — cuánto se guarda en memoria una query **sin usar**

- Cuando **ningún** componente monta una query (se desmontó la pantalla), esa entrada queda "inactiva".
- Tras `gcTime` (default **5 min**) React Query la **borra** de memoria (garbage collection).
- Si volvés a la pantalla **antes** de ese tiempo: ves los datos viejos al instante y se revalidan en segundo plano. Si volvés **después**: `isPending` de nuevo, carga desde cero.

```tsx
useQuery({
  queryKey: ["debtors", "detail", id],
  queryFn: () => apiFetch<Debtor>(`/debtors/${id}`),
  staleTime: 60_000,   // fresco 1 min
  gcTime: 10 * 60_000, // guardá el detalle 10 min aunque cierre el modal
});
```

> [!NOTE] La diferencia en una frase
> `staleTime` = "¿tengo que volver a pedirlo?"
> `gcTime` = "¿cuánto lo guardo cuando ya nadie lo mira?"
> `staleTime` es casi siempre lo que querés ajustar. `gcTime` se toca poco.

---

## 7. Refetch: cuándo se vuelve a pedir

Con datos **stale**, React Query re-pide automáticamente en estos eventos (todos configurables, y todos ignoran datos que aún estén `fresh`):

| Opción                  | Default fábrica | En el proyecto | Dispara refetch cuando...                  |
| ----------------------- | --------------- | -------------- | ----------------------------------------- |
| `refetchOnMount`        | `true`          | `true`         | se monta un componente con esa query      |
| `refetchOnWindowFocus`  | `true`          | **`false`**    | el usuario vuelve a la pestaña            |
| `refetchOnReconnect`    | `true`          | `true`         | se recupera la conexión                   |
| `refetchInterval`       | `false`         | `false`        | polling cada N ms                         |

```tsx
// Polling: un dato que querés "en vivo"
useQuery({
  queryKey: ["collections", "processing-status", batchId],
  queryFn: () => apiFetch<Status>(`/batches/${batchId}/status`),
  refetchInterval: (query) =>
    query.state.data?.done ? false : 3000, // cada 3s hasta que termine
});
```

Refetch manual:

```tsx
const { refetch, isRefetching } = useQuery({ ... });

<Button onClick={() => refetch()} disabled={isRefetching}>
  {isRefetching ? <Spinner /> : "Actualizar"}
</Button>
```

---

## 8. `useMutation`: escribir datos

`useQuery` es para **leer** (GET). Para **crear / actualizar / borrar** (POST/PUT/PATCH/DELETE) se usa `useMutation`. No corre solo: lo disparás vos con `.mutate()`.

```tsx
import { useMutation, useQueryClient } from "@tanstack/react-query";
import { toast } from "sonner";
import { apiFetch, ApiError } from "@/lib/apiClient";

function useCreateDebtor() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: (payload: NewDebtor) =>
      apiFetch<Debtor>("/debtors", { method: "POST", body: payload }),

    onSuccess: (created) => {
      toast.success(`Deudor ${created.name} creado`);
      queryClient.invalidateQueries({ queryKey: ["debtors", "list"] });
    },

    onError: (error) => {
      const msg =
        error instanceof ApiError ? error.message : "No se pudo crear el deudor";
      toast.error(msg);
    },
  });
}
```

Uso en el componente:

```tsx
function NewDebtorForm() {
  const createDebtor = useCreateDebtor();

  const onSubmit = (values: NewDebtor) => {
    createDebtor.mutate(values, {
      onSuccess: () => form.reset(), // callbacks por-llamada, además de los del hook
    });
  };

  return (
    <form onSubmit={form.handleSubmit(onSubmit)}>
      {/* ...campos... */}
      <Button type="submit" disabled={createDebtor.isPending}>
        {createDebtor.isPending ? <Spinner /> : "Crear"}
      </Button>
    </form>
  );
}
```

### 8.1 Lo que devuelve

```tsx
const {
  mutate,       // (vars, { onSuccess, onError }?) => void   -- fire and forget
  mutateAsync,  // (vars) => Promise<data>  -- devuelve promesa (para await / try-catch)
  isPending,    // la mutación está corriendo
  isSuccess,
  isError,
  error,
  data,         // lo que devolvió mutationFn
  reset,        // limpia isSuccess/isError/data/error
} = useMutation({ ... });
```

- **`mutate`**: no devuelve nada, no lanza. Manejás el resultado en callbacks. Es lo normal.
- **`mutateAsync`**: devuelve promesa y **lanza** en error. Úsalo solo si necesitás encadenar (`await` de varias mutaciones); acordate del `try/catch`.

### 8.2 Orden de los callbacks

Si definís el mismo callback en el hook **y** en `.mutate()`, corren **ambos**: primero el del hook, después el de la llamada. `onSettled` corre siempre al final (éxito o error).

### 8.3 Mapear errores de servidor al formulario

```tsx
onError: (error) => {
  if (error instanceof ApiError && error.status === 409) {
    // error del backend en un campo puntual -> inline con RHF
    form.setError("documentId", { message: "Ya existe un deudor con ese documento" });
    return;
  }
  toast.error("Error inesperado"); // el resto, global
},
```

---

## 9. `invalidateQueries` y actualizar la caché

Tras una mutación, la caché de `useQuery` quedó **desactualizada**. Tres estrategias, de más simple a más quirúrgica:

### 9.1 Invalidar (lo que usás el 90% del tiempo)

Marca queries como stale y re-pide las que estén montadas.

```tsx
const queryClient = useQueryClient();

// Coincidencia por PREFIJO: invalida ["debtors"], ["debtors","list"],
// ["debtors","list",{...}], ["debtors","detail",id]... todo lo que empiece igual.
queryClient.invalidateQueries({ queryKey: ["debtors"] });

// Solo las listas
queryClient.invalidateQueries({ queryKey: ["debtors", "list"] });

// Exacta
queryClient.invalidateQueries({ queryKey: ["debtors", "list"], exact: true });
```

> [!TIP]
> Por eso la convención de keys jerárquicas importa: `["debtors", "list", filters]` te permite invalidar **todas** las listas de deudores con `["debtors", "list"]`, sin importar el filtro.

### 9.2 Escribir la caché a mano (`setQueryData`)

Actualización instantánea sin ir al servidor. Útil cuando la respuesta de la mutación ya trae el objeto final.

```tsx
onSuccess: (updated) => {
  // reemplaza el detalle
  queryClient.setQueryData(["debtors", "detail", updated.id], updated);

  // parchea la lista sin refetch
  queryClient.setQueryData<Debtor[]>(["debtors", "list"], (old) =>
    old?.map((d) => (d.id === updated.id ? updated : d)),
  );
},
```

### 9.3 Optimistic update (avanzado)

Actualizás la UI **antes** de que responda el servidor, y revertís si falla.

```tsx
useMutation({
  mutationFn: (patch: DebtorPatch) =>
    apiFetch<Debtor>(`/debtors/${patch.id}`, { method: "PATCH", body: patch }),

  onMutate: async (patch) => {
    await queryClient.cancelQueries({ queryKey: ["debtors", "detail", patch.id] });
    const previous = queryClient.getQueryData<Debtor>(["debtors", "detail", patch.id]);
    queryClient.setQueryData<Debtor>(["debtors", "detail", patch.id], (old) =>
      old ? { ...old, ...patch } : old,
    );
    return { previous }; // pasa como `context` a onError / onSettled
  },

  onError: (_err, patch, context) => {
    // rollback
    queryClient.setQueryData(["debtors", "detail", patch.id], context?.previous);
    toast.error("No se pudo guardar, se revirtió el cambio");
  },

  onSettled: (_data, _err, patch) => {
    queryClient.invalidateQueries({ queryKey: ["debtors", "detail", patch.id] });
  },
});
```

Empezá siempre con **9.1 (invalidar)**. Pasá a optimistic solo si la latencia molesta.

---

## 10. Queries dependientes y condicionales

`enabled` decide si la query corre. Mientras es `false`, la query queda `pending` y no pide nada.

```tsx
// No pidas el detalle hasta tener un id seleccionado
const { data: selectedId } = useSomething();

const detail = useQuery({
  queryKey: ["debtors", "detail", selectedId],
  queryFn: () => apiFetch<Debtor>(`/debtors/${selectedId}`),
  enabled: !!selectedId, // <- clave
});
```

```tsx
// Encadenar: primero el usuario, después sus cobranzas
const user = useQuery({
  queryKey: ["auth", "me"],
  queryFn: () => apiFetch<User>("/me"),
});

const collections = useQuery({
  queryKey: ["collections", "by-user", user.data?.id],
  queryFn: () => apiFetch<Collection[]>(`/users/${user.data!.id}/collections`),
  enabled: !!user.data?.id,
});
```

> [!WARNING]
> Con `enabled: false` el estado inicial es `isPending: true` pero **sin** fetch en curso. Si querés distinguir "no arrancó" de "cargando", mirá `isLoading` (que es `isPending && isFetching`) o directamente el flag del que depende (`!!selectedId`).

---

## 11. Paginación y listas con filtros

El filtro va en la `queryKey`. Al cambiarlo, cambia la key → nueva query → por defecto se ve `isPending` (pantalla vacía) un instante. `placeholderData` lo evita:

```tsx
import { useQuery, keepPreviousData } from "@tanstack/react-query";

function DebtorTable() {
  const filters = useCollectionsStore((s) => s.filters); // zustand: estado de cliente
  const [page, setPage] = useState(1);

  const query = useQuery({
    queryKey: ["debtors", "list", { ...filters, page }],
    queryFn: () =>
      apiFetch<Paginated<Debtor>>(
        `/debtors?page=${page}&status=${filters.status}&q=${filters.search}`,
      ),
    placeholderData: keepPreviousData, // mantené la página anterior mientras carga la nueva
  });

  return (
    <div className="relative">
      {query.isFetching && <TopBarSpinner />}       {/* recarga sutil */}
      <Table data={query.data?.items ?? []} />
      <Pagination
        page={page}
        total={query.data?.totalPages ?? 1}
        onChange={setPage}
        // mientras isPlaceholderData, la data es de la página vieja
        disabled={query.isPlaceholderData}
      />
    </div>
  );
}
```

- `keepPreviousData`: entre cambios de key, `data` sigue siendo la anterior hasta que llega la nueva. `query.isPlaceholderData` te dice si lo que ves es "prestado".
- Combina con `staleTime` para no repedir páginas ya visitadas al volver a ellas.

---

## 12. El patrón del proyecto: capa `api/` + hook

> [!IMPORTANT] Convención BANHCAFE
> Los componentes **nunca** llaman `apiFetch` ni `useQuery` con la `queryFn` inline desperdigada. Se separa en dos capas dentro de la feature.

### Capa 1 — funciones de red puras (`features/<x>/api/`)

```ts
// src/features/debtors/api/debtorsApi.ts
import { apiFetch } from "@/lib/apiClient";
import type { Debtor, NewDebtor, DebtorFilters, Paginated } from "../types";

export function getDebtors(filters: DebtorFilters) {
  const params = new URLSearchParams({
    status: filters.status,
    q: filters.search,
    page: String(filters.page),
  });
  return apiFetch<Paginated<Debtor>>(`/debtors?${params}`);
}

export function getDebtor(id: string) {
  return apiFetch<Debtor>(`/debtors/${id}`);
}

export function createDebtor(payload: NewDebtor) {
  return apiFetch<Debtor>("/debtors", { method: "POST", body: payload });
}
```

Sin React Query acá. Se pueden testear solas.

### Capa 2 — hooks que envuelven Query (`features/<x>/hooks/`)

```ts
// src/features/debtors/hooks/useDebtors.ts
import { useQuery, useMutation, useQueryClient, keepPreviousData } from "@tanstack/react-query";
import { toast } from "sonner";
import { ApiError } from "@/lib/apiClient";
import { getDebtors, getDebtor, createDebtor } from "../api/debtorsApi";
import { debtorKeys } from "../api/queryKeys";
import type { DebtorFilters, NewDebtor } from "../types";

export function useDebtors(filters: DebtorFilters) {
  return useQuery({
    queryKey: debtorKeys.list(filters),
    queryFn: () => getDebtors(filters),
    placeholderData: keepPreviousData,
  });
}

export function useDebtor(id: string) {
  return useQuery({
    queryKey: debtorKeys.detail(id),
    queryFn: () => getDebtor(id),
    enabled: !!id,
  });
}

export function useCreateDebtor() {
  const qc = useQueryClient();
  return useMutation({
    mutationFn: (payload: NewDebtor) => createDebtor(payload),
    onSuccess: (created) => {
      toast.success(`Deudor ${created.name} creado`);
      qc.invalidateQueries({ queryKey: debtorKeys.lists() });
    },
    onError: (e) =>
      toast.error(e instanceof ApiError ? e.message : "No se pudo crear el deudor"),
  });
}
```

### Capa 3 — el componente solo consume

```tsx
// src/features/debtors/components/DebtorTable.tsx
import { useDebtors } from "../hooks/useDebtors";

export function DebtorTable({ filters }: { filters: DebtorFilters }) {
  const { data, isPending, isError, error } = useDebtors(filters);

  if (isPending) return <Spinner />;
  if (isError) return <ErrorState message={error.message} />;
  return <Table rows={data.items} />;
}
```

Y el barrel expone solo lo público:

```ts
// src/features/debtors/index.ts
export { DebtorTable } from "./components/DebtorTable";
export { useDebtors, useDebtor, useCreateDebtor } from "./hooks/useDebtors";
export type { Debtor, DebtorFilters } from "./types";
```

---

## 13. DevTools

Panel flotante que muestra cada query, su estado, sus datos y su timeline.

```bash
pnpm add -D @tanstack/react-query-devtools
```

```tsx
// src/app/providers.tsx
import { ReactQueryDevtools } from "@tanstack/react-query-devtools";

<QueryClientProvider client={queryClient}>
  {children}
  <Toaster richColors position="top-center" />
  {import.meta.env.DEV && <ReactQueryDevtools initialIsOpen={false} />}
</QueryClientProvider>;
```

Se excluye del bundle de producción con el guard `import.meta.env.DEV`. En el panel ves: si una query está `fresh`/`stale`/`fetching`/`inactive`, cuántos observadores tiene, el contenido de `data`, y botones para invalidar / refetch / resetear.

---

## 14. Errores comunes

> [!DANGER] Los clásicos
> - **Crear `new QueryClient()` en el render** en vez de `useState(() => ...)` → se pierde la caché en cada render.
> - **Faltan variables en la `queryKey`**: la `queryFn` usa `id` pero la key es `["debtors","detail"]` fija → al cambiar `id` ves datos del deudor anterior.
> - **`queryFn` que no lanza**: si tu fetch no hace `throw` en respuestas 4xx/5xx, React Query cree que fue éxito. `apiFetch` ya lanza `ApiError`; si usás otra cosa, encargate.
> - **Meter datos de servidor en zustand / `useState`** "para tenerlos a mano" → dos fuentes de verdad que se desincronizan. Si lo necesitás en otro lado, llamá al mismo hook: React Query deduplica.
> - **Llamar `mutate` en render** en vez de en un handler → loop infinito.
> - **Esperar que `useQuery` se dispare con un botón**: no. `useQuery` corre solo; para acciones on-demand es `useMutation` o `refetch()`.
> - **`enabled: false` y esperar que `refetch()` no funcione**: sí funciona, `refetch()` ignora `enabled`.
> - **`onSuccess` / `onError` en `useQuery`**: se **eliminaron** en v5. Para reaccionar a datos de una query, usá un `useEffect` sobre `data`, o mové el efecto a la mutación que los cambió.

---

## 15. Checklist

Al agregar data-fetching a una pantalla:

- [ ] ¿Es estado de **servidor**? → React Query. ¿De cliente? → `useState` / zustand.
- [ ] Función de red pura en `features/<x>/api/`, usando `apiFetch<T>`.
- [ ] Hook en `features/<x>/hooks/` que envuelve `useQuery` / `useMutation`.
- [ ] `queryKey` con prefijo de feature y **todas** las variables que usa la `queryFn`.
- [ ] `staleTime` acorde a qué tan seguido cambia el dato (default 30s del proyecto).
- [ ] Estados cubiertos: `isPending` (skeleton/spinner), `isError` (mensaje con `error.message`), `isFetching` (indicador sutil en recargas).
- [ ] Mutaciones: `onSuccess` invalida las queries afectadas (`invalidateQueries` por prefijo).
- [ ] Errores de servidor: `form.setError` si son de campo, `toast.error` si son globales.
- [ ] Botón de submit `disabled={mutation.isPending}` con `<Spinner />`.
- [ ] Export solo por el `index.ts` de la feature.
- [ ] `pnpm lint` y `pnpm build` pasan.

---

## Referencias

- Docs oficiales: https://tanstack.com/query/latest/docs/framework/react/overview
- "Practical React Query" — TkDodo (blog del maintainer): https://tkdodo.eu/blog/practical-react-query
- Guía del proyecto: [[BANHCAFE Collections — Guía del proyecto]] → sección *Datos, formularios y validación*
- Relacionado: [[Estados RTK Query]], [[Global State]]
