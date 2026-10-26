## PERMISOS EN BASE A ESTADOS
---
### Estatus 1 (solicitado)
#### Creador:
- Puede crear
- No le debe aparecer el botón de aprobar
- Puede editar todos los campos
- No le deben aparecer los campos:
- Reason for rejection
### Estatus 2
#### Revisión:
- Le debe aparecer el botón de aprobar
- Puede aprobar en el nivel 1 (revisión)  si puede aprobar, y es "solicitados" o "revisión" el estado
- Únicamente puede editar el campo "Reason for rejection"
- Si se rechaza vuelve al estatus 1
### Estatus 3
#### Autorizar:
- Le debe aparecer el botón de aprobar
- Puede aprobar en el nivel 2 (autorización) si puede aprobar, y si el estado es "autorización"
- Únicamente puede editar el campo "Reason for rejection"
- Si se rechaza vuelve al estatus 1
- Puede anular
- Si aprueba, puede pagar (botón) Si el status es aprobado y si tiene permiso para mostrar este botón