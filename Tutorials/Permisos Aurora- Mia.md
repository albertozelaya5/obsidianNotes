### Como implementar los de Aurora

```tsx
const { getScreenFiltered, getPermissionFilterd: getPermissionFiltered, isScreenChecked } = useScreensPermissions();

useEffect(() => getScreenFiltered(pathname), [pathname]);

//* Mientras no se resuelva la pantalla no se sabe si hay permisos: evita el flash de /403.

if (!isScreenChecked) return null;

//* Se deriva en cada render (no de `hasReadPermissions`, que se actualiza un render tarde)

//* para no disparar la query sin permiso de lectura.

if (getPermissionFiltered("reading") !== true) return <Navigate to="/403" />;
```

1. Primero se crean los recursos a MIA, dentro de los servicios, en este caso "Aurora"
2. El recurso devuelve una ruta con acciones (en cada recurso necesario se debe definir minimo una accion)
3. Al consumir la API, nos da un array anidado que, en cada objeto, hay un array de "actions", y en base a las que existen podemos validar - si el array "actions" viene vacio, redirigir a 403, si se tiene un action llamado "delete" que pueda ver el boton de borrar, etc
4. Cada action es un booleano
### Como implementar los de Mia

Hook: `src/hooks/useMiaScreenPermissions.ts`. Busca la pantalla actual en `screensMia` (store `auth`) y expone sus permisos.

#### Retorna

| Propiedad | Tipo | Descripción |
|---|---|---|
| `screen` | `MiaPage \| null` | Pantalla encontrada. `null` al inicio o si no existe. |
| `hasReadPermissions` | `boolean` | `true` por defecto; pasa a `false` si `getScreenFiltered` no encuentra la pantalla. |
| `getScreenFiltered(pathname, params?)` | `function` | Busca la pantalla por `metadata.path` y actualiza `screen` y `hasReadPermissions`. |
| `getPermissionFiltered(permission)` | `function` | `true` si `screen.resourceActions` incluye un `actionName` igual a `permission`. `false` si no hay `screen`. |

#### Uso básico

```tsx
const { pathname } = useLocation();

const {
  screen,
  hasReadPermissions: hasMiaPermissions,
  getScreenFiltered: screenFilterMia,
  getPermissionFiltered,
} = useMiaScreenPermissions();

useEffect(() => {
  screenFilterMia(pathname);
}, [pathname]);

if (!screen || !hasMiaPermissions) return <Navigate to="/403" />;
```

#### Rutas dinámicas (con `params`)

Si la ruta lleva ids numéricos (ej. `/it/requirements/15/details/3`), pasa los nombres de los params como segundo argumento. Cada segmento numérico del `pathname` se reemplaza, en orden, por el param, y se compara contra `metadata.path` (ej. `/it/requirements/:id/details/:detailId`).

```tsx
useEffect(() => {
  screenFilterMia(pathname, [":id", ":detailId"]);
}, [pathname]);
```

#### Permisos por acción

```tsx
const canCreate = getPermissionFiltered("Crear");
const canEdit = getPermissionFiltered("Editar");
const canDelete = getPermissionFiltered("Eliminar");

{canCreate && <Button text="Agregar" />}
```

> `getPermissionFiltered` depende de `screen`, así que solo devuelve `true` después de que `getScreenFiltered` haya encontrado la pantalla.

#### Notas

- `screen` inicia en `null`, no `undefined`: valida con `!screen`, no con `screen === undefined`.
- En el primer render `hasReadPermissions` es `true` y `screen` es `null`; evita mostrar contenido hasta que `screen` exista.
- El nombre de `permission` debe coincidir exactamente con `actionName` en `resourceActions`.
