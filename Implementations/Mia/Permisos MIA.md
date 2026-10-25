# Permisos de MIA — guía de implementación

Resumen de cómo `BANHCAFE-ReactInternalWebApp` valida permisos con MIA
(`src/hooks/useMiaScreenPermissions.ts`) y cómo replicarlo en otra app.

## Flujo en 30 segundos

```
Login (submit)
  └─ GET /backOffice/MIA/servicePermissions?userName=X&serviceId=1
       └─ hasAccess === 1 ? → continúa el login normal (POST /auth/login + OTP)
                          : → toast "Usuario no posee acceso…"
  └─ al terminar el login → se guardan permissionsJson.resources
                            (store + sessionStorage "screensMia")

Cada pantalla
  └─ useMiaScreenPermissions()
       ├─ getScreenFiltered(pathname)       → busca la pantalla por metadata.path
       ├─ hasReadPermissions === false      → <Navigate to="/403" />
       └─ getPermissionFiltered("approve")  → muestra/oculta botones
```

Regla clave: **si la pantalla está en `resources`, el usuario puede verla** (lectura). Las acciones
extra (`writing`, `approve`, `decline`…) son los `resourceActions` de esa pantalla.

## 1. La API

| Método | Endpoint                               | Params                           | Headers                            |
| ------ | -------------------------------------- | -------------------------------- | ---------------------------------- |
| `GET`  | `/backOffice/MIA/servicePermissions`   | `userName`, `serviceId` (app = 1) | `Authorization: Bearer …`, `apiKey` |

> `serviceId` identifica la aplicación dentro de MIA. Otra app tendrá **su propio `serviceId`**.

Respuesta (lo que importa):

```jsonc
{
  "data": {
    "hasAccess": 1, // 0 = sin acceso a la app → bloquear login
    "permissionsJson": {
      "resources": [
        {
          "resourceId": 10,
          "resourceName": "Aprobaciones",
          "metadata": { "path": "/finanzas/aprobaciones", "programId": 3 },
          "resourceActions": [
            { "actionId": 1, "actionName": "approve", "metadata": {} },
            { "actionId": 2, "actionName": "decline", "metadata": {} }
          ]
        }
      ]
    }
  }
}
```

En la app original se llama en `src/pages/auth/components/LoginForm.tsx` **antes** del login
(`onSubmit` → `getPermissions(...)`), y la respuesta se guarda en el store con `loginMia(data)`
cuando el OTP termina OK (`src/pages/auth/features/auth-slice.ts`).

## 2. Tipos

```ts
export interface MiaResourceAction {
  actionId: number;
  actionName: string; // "writing", "approve", "decline", …
  metadata: Record<string, unknown>;
}

export interface MiaPage {
  resourceId: number;
  resourceName: string;
  resourceActions: MiaResourceAction[];
  metadata: { id?: number | string; programId?: number; path?: string; tabName?: string };
}

export interface MiaPermissionsResponse {
  data: { hasAccess: 0 | 1; permissionsJson: { resources?: MiaPage[] } };
}
```

## 3. Guardar los permisos (store)

Original con Redux; el equivalente en zustand:

```ts
// features/auth/store/authStore.ts
import { create } from "zustand";
import type { MiaPage, MiaPermissionsResponse } from "../types";

const KEY = "screensMia";

interface AuthState {
  screensMia: MiaPage[] | null;
  setMiaPermissions: (res: MiaPermissionsResponse) => void;
  clearSession: () => void;
}

export const useAuthStore = create<AuthState>((set) => ({
  // Se rehidrata para que un F5 no pierda los permisos
  screensMia: JSON.parse(sessionStorage.getItem(KEY) ?? "null"),

  setMiaPermissions: (res) => {
    const resources = res?.data?.permissionsJson?.resources;
    if (res?.data?.hasAccess === 0 || !resources?.length) return;
    sessionStorage.setItem(KEY, JSON.stringify(resources));
    set({ screensMia: resources });
  },

  clearSession: () => {
    sessionStorage.removeItem(KEY); // ¡también en el logout!
    set({ screensMia: null });
  },
}));
```

Llamada en el login:

```ts
const res = await getMiaPermissions({ userName, serviceId: SERVICE_ID });
if (res.data.hasAccess !== 1) return toast.error("Usuario no posee acceso…");
await login(form);              // login normal
setMiaPermissions(res);         // solo cuando el login fue exitoso
```

## 4. El hook

Versión original (simplificada): guarda la pantalla en `useState` y hay que llamarla en un `useEffect`.

```ts
export const useMiaScreenPermissions = () => {
  const screensMia = useAuthStore((s) => s.screensMia);
  const [screen, setScreen] = useState<MiaPage | null>(null);
  const [hasReadPermissions, setHasReadPermissions] = useState(true);

  const getScreenFiltered = (pathname = "", params?: string[]) => {
    // Rutas dinámicas: /clientes/123 → /clientes/:id (cada segmento numérico se cambia por params[i])
    let i = 0;
    const path = params
      ? pathname.split("/").map((seg) => (seg && !isNaN(+seg) ? params[i++] : seg)).join("/")
      : pathname;

    const found = screensMia?.find((s) => s.metadata.path === path) ?? null;
    setScreen(found);
    setHasReadPermissions(!!found);
  };

  const getPermissionFiltered = (action = "") =>
    !!screen?.resourceActions.some((a) => a.actionName === action);

  return { screen, hasReadPermissions, getScreenFiltered, getPermissionFiltered };
};
```

**Versión recomendada para una app nueva** (derivada, sin `useEffect` ni parpadeo):

```ts
export function useMiaPermissions(routePattern?: string) {
  const { pathname } = useLocation();
  const screensMia = useAuthStore((s) => s.screensMia);
  const path = routePattern ?? pathname; // p. ej. "/clientes/:id"

  const screen = useMemo(
    () => screensMia?.find((s) => s.metadata.path === path) ?? null,
    [screensMia, path],
  );

  const can = useCallback(
    (action: string) => !!screen?.resourceActions.some((a) => a.actionName === action),
    [screen],
  );

  return { screen, canRead: !!screen, can };
}
```

> Para rutas dinámicas, pasar el patrón de la ruta (`useMatches()` / el `path` del router)
> en vez de reconstruirlo desde los números de la URL.

## 5. Uso al validar

**Proteger la pantalla** (sin lectura → 403):

```tsx
// Original
const { pathname } = useLocation();
const { getScreenFiltered, hasReadPermissions, getPermissionFiltered } = useMiaScreenPermissions();
useEffect(() => getScreenFiltered(pathname), [pathname]);
if (!hasReadPermissions) return <Navigate to="/403" />;

// Recomendado
const { canRead, can } = useMiaPermissions();
if (!canRead) return <Navigate to="/403" />;
```

**Mostrar/ocultar acciones:**

```tsx
{can("approve") && <Button onClick={approve}>Aprobar</Button>}
{can("decline") && <Button onClick={decline}>Rechazar</Button>}
<DataGrid editing={{ allowUpdating: can("writing") }} />
```

**Ruta dinámica** (`/banca/clientes/123` registrada en MIA como `/banca/clientes/:id`):

```ts
getScreenFiltered(pathname, [":id"]);   // original
useMiaPermissions("/banca/clientes/:id"); // recomendado
```

**Subcomponentes/modales** de la misma pantalla: llaman al hook igual (con el mismo `pathname`),
no hace falta pasar los permisos por props.

## 6. Ojo con esto

- `metadata.path` en MIA debe coincidir **exacto** con la ruta del front (mayúsculas, sin `/` final).
- En el hook original `hasReadPermissions` arranca en `true`: la pantalla se pinta un instante antes de
  validar. La versión derivada evita eso.
- Ocultar botones es solo UX: **el backend debe validar el permiso** en cada endpoint.
- Limpiar `screensMia` en el logout (store + `sessionStorage`).
- `src/pages/auth/service/auth.ts` de la app original tiene un **JWT hardcodeado** en
  `prepareHeaders`; en la nueva app usar el token de la sesión, nunca uno fijo en el código.
