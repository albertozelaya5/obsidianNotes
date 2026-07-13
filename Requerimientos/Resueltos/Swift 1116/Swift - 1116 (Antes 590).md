---
tags:
  - features
  - RESUELTO
---
> [!Tareas por hacer]
> - [x] hacer seccion 4
> - [x] Arreglar el paylaod
> - [x] clearingAccountId y clientType son requeridos, borrar useGLAccount
> - [x] Fecha valor de liquidacion no va en plantillas
> - [x] Poner en editar y plantillas - ahi es todo lo mismo pero sin el campo plantilla
> - [x] Mandar los mismos ids de los roles en los actores
> - [x] poner preview de xml
> - [x] boton de preview xml arriba del xml, si si no se manda, solo se muestra el preview actualizado
> - [x] validacion de los campos anidados address
> - [x] prioridad formatear
> - [x] tipo de mensaje cuando llegue ejecutarse el xml

- que cuando me de un error en actors, mapear el errors y ponerlo en el input
- O, que cuando me de errors en un actor, marcar ese actor en rojo border, y poner los errores en el ActorCard.tsx


En console.log(createError?.data?.validationErrors); yo tengo este objeto

```json
{
    "PaymentRequest.Actors[0].ClientType": [
        "El tipo de cliente es requerido.",
        "El tipo de cliente es requerido para el actor 'Nuevo Cliente / New Party'"
    ],
    "PaymentRequest.Actors[1].ClientType": [
        "El tipo de cliente es requerido."
    ],
    "PaymentRequest.Actors[2]": [
        "Debe proporcionar BIC o ABA cuando se especifica información bancaria"
    ],
    "PaymentRequest.Actors[0].Address.StreetName": [
        "Calle es requerida."
    ],
    "PaymentRequest.Actors[0].Address.TownName": [
        "Ciudad es requerida."
    ],
    "PaymentRequest.Actors[0].Address.CountryId": [
        "Pais es requerido."
    ]
}
```

Quiero que cuando devuelva eso, el border cambiarlo de color a `colorError`, y poner los erores correspondientes en el actor correspondiente

por ejemplo , si hay un error en actors[0], que en ese actor se muestren esos erores, los puedes mostrar en el componente MissingBadge, o puedes hacer una ul > li para marcar esos errores en orden

> [!IMPORTANT]
> -  Incoming = "Enviadas", Outgoing = "Recibidas"
> - fix/swift-req-590
> - feat/swift-req-590-dev
> - feat/swift-req-590-qa

> [!IMPORTANT] correcciones de validacion
> - fix/swift-1116
> - fix/swift-1116-dev
> -  fix/swift-1116-qa

> [!IMPORTANT]
> - feat/swift-date-updated
> - feat/swift-date-updated-dev
> - feat/swift-date-updated-qa

[[Recursos - Swift]]

---
## Roles actuales
- Comite
- Admin
- Desarrollador
- QA
- Usuario certificador
