
### Requerimientos y Solicitudes

> Post

- Admin: cambiar y modificar todo 
	  manageRequirement, descriptionManagement
- User normal o miembro: solo lectura
- Responsables: solo lectura

> Editar

- Admin: Estado - admin editState , Categoria editCategory, assignInternalPriority
- Dueño: editCategory, cambiar la descripción, assignInternalPriority
- Responsable: editCategory, cambiar la descripción, descriptionManagement

### Tabs

#### Detalles
- **Todos**: ver → `viewDetails`
#### Evaluación
- **Todos menos miembro**: ver → `viewEvaluation`
#### Miembros
- **Todos**: ver  
- **Admin**: CRUD → `manageMembers`
#### Requisitos
- **Todos**: crear, editar, eliminar
#### Documentación 
> [!IMPORTANT]
> Parte de Backend
- **Todos**: crear y ver  
- **Dueño**: editar y eliminar **solo los documentos que subió** → `canOwnerEditDocs`, `canOwnerDeleteDocs`
#### Comentarios
> [!IMPORTANT]
> Parte de Backend
- **Todos**: crear y ver  
- **Dueño**: editar y eliminar **solo los comentarios que subió** → `canOwnerEditComments`, `canOwnerDeleteComments`
#### Planificación
- **Dueño y responsables**: ver → `viewPlanning`
#### Post (primer nivel)
- **Admin**: crear/editar → `planningFirstLevel`
#### Post (segundo nivel)
- **Programador o QA**: crear/editar → `planningSecondLevel`

## Roles de Requerimientos

- RequirementADMIN: manageRequirement, descriptionManagement, reading, delete, editState, editCategory, assignInternalPriority, viewEvaluation, manageMembers, planningFirstLevel

- RequirementOwner: editCategory, editDescription, assignInternalPriority, descriptionManagement,viewEvaluation, viewPlanning

- RequirementUser: viewEvaluation, planningSecondLevel
Programmer | QA

- RequirementMember: 

- RequirementResponsables: descriptionManagement, editDescription, editCategory, viewPlanning

## Tabla de estados del requerimiento


| **Estado**              | **Descripción / Acción**               | **Roles que pueden realizarlo**     |
| ----------------------- | -------------------------------------- | ----------------------------------- |
| Nuevo         	  | Nueva Solicitud                        | Usuario certificador, Admin, Comité |
| Aprobado                | Nuevo requerimiento                    | Admin, Comité                       |
| Rechazado               | Pendiente de corrección por el Usuario | Admin, Comité, QA                   |
| En QA                   | En revisión por QA                     | QA, Admin, Comité                   |
| En certificación        | En proceso de certificación técnica    | Usuario certificador, QA, Admin     |
| Certificado             | Aprobado y certificado correctamente   | QA, Admin                           |
| Detenido                | No se ha certificado en mucho tiempo   | Admin, Comité                       |
| Trasladado a Producción | Listo y migrado a entorno productivo   | Admin, Comité, Q                    

New, Approved, Rejected, In QA, In Certification, Certified, Stopped, Moved to Production

## Grupos actuales
| **Rol / Grupo**      | **Nombre técnico del rol** | **Nombre de la política**  | **Descripción de la política**                                                     |
| -------------------- | -------------------------- | -------------------------- | ---------------------------------------------------------------------------------- |
| Comité               | 		            | `committee-policy`         | Permite revisar, aprobar o rechazar solicitudes en las etapas de Comité.           |
| Administrador        | `REQUIREMENT_ADMIN`        | `requirement-admin-policy` | Control total sobre el flujo: creación, edición, asignación y envío a QA o Comité. |
| Desarrollador        | `REQUIREMENT_DEV`          | `requirement-dev-policy`   | Puede trabajar sobre requerimientos técnicos, recibirlos y marcarlos como listos.  |
| QA (Calidad)         | `REQUIREMENT_QA`           | `requirement-qa-policy`    | Puede revisar solicitudes, aprobar o rechazar en la etapa de QA.                   |
| Usuario certificador | `REQUIREMENT_USER`         | `requirement-user-policy`  | Puede crear nuevas solicitudes, corregir observaciones y enviar a QA.              |

## Permisos de las politicas actuales
| **Role**                    | **Permissions**                                                                                                                                                                                                          |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **REQUIREMENT-QA**          | `sendToQa`                                                                                                                                                                                                               |
| **REQUIREMENT-Owner**       | `descriptionManagement`, `editCategory`, `assignInternalPriority`, `viewPlanning`, `viewEvaluation`                                                                                                                      |
| **REQUIREMENT-User**        | `planningSecondLevel`, `viewEvaluation`                                                                                                                                                                                  |
| **REQUIREMENT-Responsable** | `descriptionManagement`, `editCategory`, `viewPlanning`                                                                                                                                                                  |
| **REQUIREMENT-Admin**       | `manageRequirement`, `descriptionManagement`, `editState`, `editCategory`, `assignInternalPriority`, `viewPlanning`, `manageMembers`, `reading`, `delete`, `planningFirstLevel`, `planningSecondLevel`, `viewEvaluation` |