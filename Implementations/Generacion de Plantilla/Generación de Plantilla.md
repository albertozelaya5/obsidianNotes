> [!IMPORTANT]
> Poliza > Generacion de Planilla
> - feat/payroll-generator
> - feat/payroll-processing
> - feat/payroll-generator-dev
> - feat/payroll-generator-qa

> [!TODO]
> - Quitar error que si viene vacio que hace falta crear plantilla
> - Cuando los filtros esten vacios, recargar, el de "Empleado"

> [!ERROR] Flujo que estaba antes
```typescript
const dispatch = useAppDispatch();

const { data, isLoading, isFetching, isSuccess } = useGetPayrollQuery({ page, size, ...queryFilters });

const hasWarnedNoTemplate = useRef(false);

useEffect(() => {
if (isSuccess && !isFetching && !hasRows && !hasWarnedNoTemplate.current) {
  hasWarnedNoTemplate.current = true;
  dispatch(
	addNotification({
	  message: ["Debe generar una plantilla para continuar"],
	  type: "error",
	}),
  );
}
}, [isSuccess, isFetching, hasRows, dispatch]);
```

statusProcess



En el componente src/pages/HumanResources/payroll/payroll-processing/PayrollList.tsx, va a haber un campo nuevo llamado `statusProcess`, en este, cuando se creen los registros mediante "Nueva", y por defecto el primer estado de `statusProcess` sera "EN PROCESO", 

Mientras este en proceso podrá => darle a "Nueva" para generar, y visualizar datos
Luego se le da click a "Enviar a aprobación" se usa el PATCH `/PolizaHTIS/Pending`=> `statusProcess` pasa a "PENDIENTE", donde podrá seguir visualizando los datos de `/PolizaHTIS/View`  y ver el GET de `/PolizaHTIS/Batch` (el boton de + nueva estara deshabilitado o no se vera de aqui en adelante)

Para ver el get de Batch, aunque es un get, se necesita enviar un campo manual llamado "batch", ese dime si es mejor dejarlo en un modal, enviarlo a otra pantalla o que ondas

Cuando este en "PENDIENTE" => se usa el PATCH `/PolizaHTIS/Pending` que seria al darle click a "Aprobar", donde en el view, el status pasa a ser "APROBADO" (por ende tambien se deshabilita o no se ve el boton de "Aprobar")

Tambien cuando este en "PENDIENTE" => tiene acceso al boton "Rechazar", (falta el endpoint), que regresaria los registros a "EN PROCESO"

En esta estado de "APROBADO" es donde tienen acceso al boton "Cerrar / Contabilizar" usando el POST `/PolizaHTIS/Accounting`, para luego el status de ese view pasar a "CERRADO"

En resumen tendrias que crear unos botones mas, e implementar nuevos endpoints, dime si todo esta claro, y ayudame con eso del batch

tambien, plantea este flujo de una mejor manera para verificar que el entendi bien a mi backend y empezar a implementar
%%%%

## Pantalla RRHH

- Generar con parámetros, visualizar, descargar y enviar a aprobación => Perfil 1 Carmen (Generador)
- GET y POST(estas seguro?) visualizar, descargar y aprobar => Perfil 2 Gerson (Aprobador)

- Pending => enviar a aprobacion 3
- Accounting => aprobar 5
- Rechazar => pending proceso, vuelve al paso 2 (1)
- Batch => get del aprobador, previa de contabilidad - Aprobador only 4
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

commit 013f2911c361318177b055e68e8c3688d4a7fa43 (HEAD -> feat/payroll-generator, origin/feat/payroll-generator)
Author: albertozelaya <ajzelaya@banhcafe.hn>
Date:   Wed Oct 7 10:12:07 2026 -0600

    fix: quit error when is no data

commit 1bd3924feb5c3d874bd3c0fe2a6f8cb38abad34e
Author: albertozelaya <ajzelaya@banhcafe.hn>
Date:   Wed Oct 7 08:57:08 2026 -0600

    feat: add new column "optional1"