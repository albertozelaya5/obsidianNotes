
>[!IMPORTANT]
> - fix/comparer-core-req-341
> - fix/comparer-core-req-341-dev
> - fix/comparer-core-req-341-qa

> [!TODO] Tareas por hacer
> - [ ] Si quieren poner el nuevo expediente manual (en el mismo modal poner el input), y al confirmar cambios llamar a un endpoint para ver si este existe => tenerlo en cuenta
> - [ ] `/backOffice/DigitalArchiveProcess/FileTimeline?fileId=133764` => poner acordion, y dentro del acordion una tabla comparativa de como estaba antes y como esta ahora => si solo existe un objeto, poner (no existen cambios ultimamente)

bd865833425c29481c0b601bc3f0674befe856a0

-- a0cd827a9e8c6f52b1a51e39300215f7a077a8b0

### SIPAD — resumen para explicar a QA

Qué es en general: SIPAD es el módulo de Archivo Digital que controla el ciclo de vida de un expediente/gestión entre agencias: se envía información desde una agencia, alguien la recibe y valida, y por otro lado se da seguimiento al proceso de digitalización del documento físico en OnBase (recibido → ingresado → escaneado → indexado → confirmado → revisado). Las tres pantallas del menú son básicamente las 3 etapas de ese flujo.

---

1. Envío de Información (/sipad/envio-de-informacion)

Es donde una agencia registra que está enviando un expediente/gestión a otra área.

---

2. Recepción de Información Enviada (/sipad/recepcion-informacion-enviada)

Es el lado receptor: quien recibe el envío revisa/valida lo que llegó.

---

3. Seguimiento Documentación Enviada (/sipad/seguimiento-documentacion-enviada)

![[Pasted image 20260825145423.png]]

![[Pasted image 20260825151511.png]]

Numero de cliente ingresar, y ver si ya se posee ese expediente en el departamento
![[Pasted image 20260825151703.png]]
Ellos no manejan caja sino expedientes

> [!TODO] Cosas que cambiar

**Seguimiento de informacion:**
- Agregar busqueda por expediente global
	
**Recepcion de informacion:**
- Que se seleccionen varios, y se pueda cambiar de estado

**Envio de informacion:**
- Que cuando se agregen errores, se vuelva a hacer la peticion para que se muestren
- Boton de cerrar validacion de errores

- Pantalla nueva de seguimientos sin expedientes => parecido, que la opcion de tipo de producto solo tenga a los que sean sin expediente, editar, crear
- Crea expedientes si o no - opcion booleana tipo de gestion, parametrizacion
- Depende del tipo producto cambia el numero de expediente => mostrar actual, y nuevo numero de expediente