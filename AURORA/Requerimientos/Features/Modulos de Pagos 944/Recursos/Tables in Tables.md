### Dentro de la tabla

```tsx
{getPermissionFilterd("writing") && (
<ButtonActionsEdit title="Items a evaluar" onClick={() => handleShowDocQuestions(data?.data)}>
  {tableDocInfo?.openTable && tableDocInfo?.id === data?.data?.procurePaymentId ? (
	<IoCloseOutline />
  ) : (
	<AiOutlineUnorderedList />
  )}
</ButtonActionsEdit>
)}
```

###  Función para mostrar la tabla
```tsx
const handleShowDocQuestions = function (row: SingleProPayments) {
if (tableDocInfo?.openTable && tableDocInfo?.id === row?.procurePaymentId)
  return setTableDocInfo((docObject) => {
	return { openTable: !docObject?.openTable, tableInfo: null, id: null };
  });

setTableDocInfo({ openTable: true, tableInfo: row?.documentsQuestions, id: row?.procurePaymentId });
};
```

### Dirigir a la tabla al dar click al botón

```tsx
const tableItems = useRef<HTMLDivElement | null>(null);

useEffect(() => {
if (tableDocInfo?.openTable && tableItems.current) {
  tableItems.current.scrollIntoView({ behavior: "smooth" });
}
}, [tableDocInfo]);
```

