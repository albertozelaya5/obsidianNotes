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

### Como implementar los de Mia

```tsx
  const {
    //
    screen,
    hasReadPermissions: hasMiaPermissions,
    getScreenFiltered: screenFilterMia,
    getPermissionFiltered,
  } = useMiaScreenPermissions();
  
	useEffect(() => {
    screenFilterMia(pathname);
  }, [pathname]);

if (screen === undefined || !hasMiaPermissions) return <Navigate to="/403" />;
```