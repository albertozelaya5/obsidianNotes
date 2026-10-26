## OBSERVACIONES VOL. 4
### Perfil comité

- [ ] ¿Se podrá modificar departamentos y plataformas?
- [x] ¿Pantalla detalle es necesario que se visualice plataformas?
- [x] Participante de comité quitar miembro
- [ ] Permitir asignar requerimiento al estado que estaba anteriormente de estado detenido - CARLOS
- [ ] No permitir enviar requerimientos de QA a desarrollo - CARLOS

### Perfil User

- [ ] Error al cargar documentación al crear solicitud - CARLOS
- [x] No permite agregar requisitos si está rechazado por comité
- [x] No hay opción de rechazar requerimiento
- [x] No permite agregar requisitos si está en certificación
- [ ] Corregir mensaje actualización de requisitos cuando ya pasó a certificado el requerimiento - CARLOS

### Perfil desarrollador

- [ ] No envía correo por la notificación de la tarea asignada - CARLOS
- [ ] Cuando se agrega subtarea no identifica la tarea como pendiente - CARLOS
	```
	http://172.20.19.11:8000/backOffice/Requirements/tasks?requirementId=845&parentTaskId=5555&showUnapprovedReq=false
	```
- [x] Ingresar validación que si hay tareas pendientes no permita enviar a certificación
- [ ] Corregir mensaje de validación de los ítems - CARLOS
- [ ] Documento solo genera un requisito - CARLOS
- [ ] Agregar rol de auditor - CARLOS

## OBSERVACIONES VOL. 3

### Nueva solicitud
- [x] Documentación solo permite agregar un archivo  
- [x] Pantalla detalle separar departamentos involucrados con comas  
- [x] No es necesario el menos 1 en días atraso del requerimiento  
- [x] Agregar usuario y fecha que ingresó requisito  

### Perfil admin
- [x] Quitar opción de recibido por IT  

### Perfil comité
- [x] Agregar buscador para requerimiento padre  
- [ ] Si se rechaza por comité, permitir agregar requisitos  - ESTO YA ESTA
- [x] Genera error: sólo un requerimiento que es proyecto puede tener una fecha de entrega  - CARLOS
- [x] Los Items solo se pueden visualizar  
- [ ] Revisar los estados en que se puede detener un requerimiento  - CARLOS
- [ ] Permite mover requerimiento de QA a desarrollo  - CARLOS

### Perfil desarrollo
- [ ] Planificación no cambia tarea a detenida  - CARLOS
- [ ] Corregir mensaje cuando no tiene Items - CARLOS
## OBSERVACIONES VOL. 2

- [x] Botón de guardar solo habilitar cuando ya están todos los campos completos  
- [x] Advertencia si no se cargó documentación – ver si ya lo hice  

### Perfil comité

- [x] Realizar consulta de la fecha cuando es proyecto – siempre mandar fecha req, en nuevo new Date()  
- [x] Marca como campo obligatorio los involucrados – error al editar req  
- [x] Fecha limite solo mostrar si es proyecto – días restantes, días atrasados si es negativo (en cards), fecha limite en detalles  
- [x] Si se rechaza por comité habilitar opción de plataformas  
- [x] Si se rechaza por comité se pueden agregar más requisitos  

### Perfil Usuario

- [x] Una vez aceptado en comité no permitir modificar campos al user (departamentos, categoría y involucrados) – desde que es aprobado en comité, solo al user que no pueda editar requerimiento 

### Certificación

- [x] Agregar opción de agregar requisito cuando está “en certificación”  
- [x] Al intentar rechazar el requerimiento genera solo requerimiento que es proyecto puede tener una fecha de entrega – siempre mandar fecha x2 “deadline”  

### Infraestructura

- [x] Al intentar trasladar a producción el requerimiento genera, solo requerimiento que es proyecto puede tener una fecha de entrega – siempre mandar fecha x2 “deadline”  

### Asistente

- [x] No permite agregar documentación – solo si está trasladado a producción  

### Pantalla detalles

- [x] No se visualizan los involucrados – en responsables  
- [x] Cambiar nombre de responsable a involucrados – en detalles  

## OBSERVACIONES VOL. 1
### Requisitos

- [x] Poner lo del requerimiento padre, solo al momento de editar

- [x] Documentación, rpg y web quitarlos de plataformas, poner Agentes (Mía), autobancos, administrativas (Mía), otras - CARLOS
- [x] En lugar de “leyes”, en categorías poner **regulatorios** - CARLOS
- [x] Documentación: pantalla opcional, al aprobarse mostrar advertencia si no se cargó documentación - CARLOS Y YO
- [x] Notificar por correo cuando el usuario tenga tareas pendientes - CARLOS
- [x] Pasar las secciones de Requerimientos a rutas
- [x] Cambiar nombre de categoría “en certificación” a **certificación usuario** en estado - CARLOS
- [x] Tipos de requisitos - CARLOS :
  - [x] Requisitos de negocio
  - [x] Requisitos de stakeholders (usuarios interesados)
  - [x] Requisitos funcionales (acciones, comportamientos, funciones específicas)
  - [x] Requisitos no funcionales (rendimiento y seguridad)
  - [x] Requisitos de transición (migración)
  - [x] Requisitos técnicos

- [x] Botón de habilitar requerimiento padre, solo con autorización de comité
- [x] Cambiar nombre select de responsables por **Involucrados**
- [x] Al crear un requerimiento, mandar Requisitos y evaluación  - CARLOS Y YO

- [x] Miembro: pantalla solo visible para requerimientos aprobados (ver si cambiar nombre)

- [x] Los que no lleven “sí” en requiere verificación son deseables
- [x] Principal es el mismo que lo crea
- [x] items => QUE SE VEA EN TODOS LOS ESTADOS
- [x] editar o crear, eliminar items=>que solo aparezca cuando el estado este en certificacion usuario

- [ ] Listado de requerimientos en dashboard admin: exportar a PDF o Excel

- [x] Hacer label de descripción más largo en documentación

- [x] Poner prioridad al momento de crear (user Mía) y en editar solo comité pueda modificarla
- [x] Cambiar nombre de “activo” a sí o no
- [x] Cambiar nombre de la pantalla de miembro a **técnico**
- [x] Miembro de comité → cambiar a **participante de comité**
- [x] Desarrollador no puede ver pantalla de miembros (opcional, validar)

- [x] Trasladado a producción: habilitarse solo para los que llenaron los ítems
- [x] En usuario → editar requerimiento: permitir dar siguiente estado o rechazarlo

### Evaluación

- [x] Objetivo en evaluación que sea manual - CARLOS - poner en los evaluationPerspectives > objectives > un campo llamado `objective`, tipo texto
- [x] En reducción y mitigación de riesgos, poner sí o no, y si es sí, mostrar la lista - YO -CARLOS - poner posible validación si es `mitigación de riesgos`
- [x] En evaluación, no permitir enviar si no se llenan todos los campos

### Roles
---
- [x] Rol para Fanys(ASISTENTE DE IT): pueda agregar documentación (solo los que estén trasladado a producción)
- [x] Que el usuario pueda rechazar un requerimiento (aceptar o no) en certificación (**hacerlo de último**)
- [x] Crear rol de **admin infra**: Ellos podrían pasar un req a trasladado a producción - SOLO ELLOS
- [x] Admin producción: solo ver certificados, número de requerimiento y estado; permitir ver detalles (no ver las otras tabs)
- [x] Auditoría: rol de consulta, puede ver todo pero solo lectura
