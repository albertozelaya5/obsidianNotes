> [!IMPORTANT]
> Poliza > Generacion de Planilla
> - feat/payroll-processing
> - feat/payroll-processing-dev
> - feat/payroll-processing-qa

## Pantalla RRHH

- Generar con parámetros, visualizar, descargar y enviar a aprobación => Perfil 1 Carmen
- GET y POST(estas seguro?) visualizar, descargar y aprobar => Perfil 2 Gerson

#### Status
- generado (se puede varias veces )
- pendiente de aprobación (no se puede generar varias veces)
- cerrado

botones:
- Enviar a aprobación - Perfil 1
- Rechazar - Perfil 2
- Aprobar - Perfil 2
- Descargar

```jsx
<Column
  showInColumnChooser={false}
  caption="Acciones"
  width="100"
  cellRender={(data, index) => (
	<div style={{ display: "flex" }}>
	  {/* {getPermissionFiltered("writing") ? ( */}
	  <ButtonActionsEdit title="Editar" onClick={() => handleUpdate(data?.data)}>
		<AiOutlineEdit />
	  </ButtonActionsEdit>
	  {/* ) : null} */}

	  {/* {getPermissionFiltered("delete") && ( */}
	  <ButtonActionsEdit
		className="error"
		title="Eliminar"
		onClick={() => {
		  handleConfirmDelete(data?.data);
		  dispatch(setOptionSelectedId(index));
		}}
>
		<AiOutlineDelete />
	  </ButtonActionsEdit>
	  {/* )} */}
	</div>
  )}
/>
```

