---
tags:
  - features
---
[[Permisos MIA]]

> [!IMPORTANT]
> - feat/payments-req-944
> - feat/payments-dev
> - feat/payments-qa
> - feat/payments-base-squash
> - feat/payments-catalogs (practicante Joel)
> - feat/payments-base-req-944 (La que va a combinar ambas ramas)

---

> [!Tareas por hacer]
>- Si el estado es "finalizado" no se puede editar
>- Que no se pueda volver a editar cuando la solicitud ya esté cerrada
>- Una orden para anular un pago ya finalizado, estado de cerrada a anulada

> [!Listado de Politicas en MIA]
>- PAYMENT-Maker
>- PAYMENT-Reviewer
>- PAYMENT-Authorizer

> Permisos
> emisión, revision, autorizado
> pendientes de aprobar y aprobadas


1.  Emitida  // Emisión
2. por revisar
3. Revisada
4. Por autorizar
5. Autorizado

## Reglas Formulario

- `Maker`: 
	- Puede crear Orden de Pago
	- Solo puede editarla si esta en "rechazada", tambien puede editar el documento fiscal
	- Si esta "rechazada", puede cambiar al siguiente estado
- `Reviewer`:
	- Puede editar orden de pago si esta "Solicitada", y lo que puede editar solo la Distribución de Pagos
	- Puede cambiar a "Rechazada", y poner una razón 
- `Authorizer`:
	- Puede editar orden de pago si esta "Por autorizar", y lo que puede editar solo es la Distribución de Pagos
	- En "Por autorizar" puede cambiar el estado a "Aprobada"
	- Cuando esta "Aprobada" puede pagar la orden, siempre que tenga Distribución de Pago

### Payment status paymentStatusId

> [!Estados de Pago]
9 > CANCELADO
8 > ANULADO
7 > PAGADA
6 > APROBADA
5 > RECHAZADA
4 > CERRADA
3 > POR AUTORIZAR
2 > SOLICITADO
 