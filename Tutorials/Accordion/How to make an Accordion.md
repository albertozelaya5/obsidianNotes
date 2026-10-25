```jsx
  const inputsDataSource = useMemo(
    function () {
      return fieldDist?.map((input, index) => {
        return { ...input, index, title: `Pago ${index + 1}` };
      });
    },
    [fieldDist]
    
    return <Accordion dataSource={inputsDataSource ?? []} displayData={DistPayment} multiple={false} />
  );
```

- `displayData`=> el componente a mostrar
- cada item del `dataSource` necesita un `title` (es lo que se ve en la cabecera del panel)

---

## Con paginación (opcional)

Props opt-in. Si NO se pasa `paginated`, el Accordion se comporta igual que
siempre (sin redux, sin `<Pagination>`).

| prop | tipo | default | para qué |
|---|---|---|---|
| `paginated` | `boolean` | `false` | activa el `<Pagination>` debajo de los paneles |
| `paginationId` | `string` | `"accordion-1"` | key de `state.ui.tableFilter[id]`. **Único por pantalla** |
| `pageSize` | `number` | `5` | filas por página inicial |
| `serverSide` | `boolean` | `false` | `false` = corta local; `true` = el backend pagina |
| `metadata` | `MetadataType` | — | metadata real del API. Obligatoria si `serverSide` |
| `isLoading` | `boolean` | `false` | sólo server-side: oculta el Pagination mientras carga |

### Local (no manda page/size a ningún API)

El Accordion recibe **todo** el array y hace `.slice()` en cliente.

```tsx
<Accordion
  dataSource={items}          // array completo
  displayData={Detail}
  paginated
  paginationId="cuentas-cliente"
  pageSize={5}
/>
```

### Server-side (manda page/size dinámicamente)

El Accordion recibe **sólo la página actual** y no la corta. El padre lee la
página/size de redux y refetch-ea. `metadata` sale del response del API.

```tsx
const { currentPage = 0, rowSelectedPerPage = 5 } = useAppSelector(
  (s) => s.ui.tableFilter["cuentas-cliente"] ?? {}
);

const { data, isFetching } = useGetAccountsQuery({
  customerId,
  page: currentPage,
  size: rowSelectedPerPage,
});

<Accordion
  dataSource={data?.data ?? []}   // sólo la página que devolvió el API
  displayData={Detail}
  paginated
  serverSide
  paginationId="cuentas-cliente"
  pageSize={5}
  metadata={data?.metadata}
  isLoading={isFetching}
/>
```

- El `paginationId` del `useAppSelector` y el del `<Accordion>` **tienen que ser el mismo**.
- `Pagination` lee `state.ui.tableFilter[id]` sin fallback → si ese `id` ya lo
  usa otra tabla en la misma pantalla, se pisan. Usar un id propio.
- `lastPage` es índice 0-based (el Pagination muestra `currentPage + 1`).
