## Auditoria - Enlistar las cosas faltantes y errores

- [x] Error al momento de agregar un contacto (al poner un nuevo memorandum)
- [x] Al crear un nuevo hallazgo, que se ordene por el último ingresado
- [ ] Hacer oportunidades de mejora (apartado mío, validar que todos los endpoints funcionen)
- [ ] Arreglar error 400 cuando no hay riesgos asociados
- [ ] Quitar los campos “en inglés” (ej. riesgo en inglés)
- [ ] Al ingresar una nueva respuesta da error: “No existe el memorandum o no está asociado al usuario”
- [x] Organizar correctamente el JSON de `OpportunityForImprovement` (GET)

## Mejoras Generales

- [ ] Arreglar que se reseteen los temas

```jsx
  useEffect(() => {
    document.body.classList.add(theme);
    return () => {
      document.body.classList.remove(theme);
    };
  });
```
## Organizar la creación de un programa

- wikiDev
- Curso de codely

## Revisar pantallas tardadas

- [ ] Pantalla de roles, mantenimiento y nuevo rol => refactorizarla
- [ ] Pantalla de menú de inicio
- [ ] Arreglar los fontSize de CSS
- [ ] Agregar nuevos iconos, optimizar librería
- [ ] Mejorar login
- [x] Mejorar pantallas de Tokens

---
### Otras mejoras
---
- Wireframe de mejoras en el sistema de requerimientos
- Que la documentacion la linkee con los commits de GitHub

Endpoint para subir imágenes en Strapi para la banca

Que tenga parámetros opcionales
Enviar imágenes, url (opcional), mas otros campos opcionales

7665
---
Contabilidad > Configuracion > Sistema de Depositos
Solo descargar un archivo
