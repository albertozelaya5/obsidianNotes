Por defecto, Yup valida cada campo de forma aislada e independiente. Cuando creas una lógica donde el campo **A** depende de **B**, y el campo **B** depende de **A**, Yup puede entrar en un **bucle infinito** (intentando validar uno al otro infinitamente hasta congelar la aplicación).

La matriz de dependencias al final del `.shape()` le avisa a Yup qué campos están amarrados entre sí para que los valide juntos de forma segura en un solo ciclo.

#### Ejemplo Corto:

```typescript
export const esquema = yup.object().shape({
  sendEmail: yup.boolean(),
  sendSms: yup.boolean().test(/* ... lógica que lee sendEmail ... */)
}, 
// MATRIZ DE DEPENDENCIAS (Al final del objeto):
[
  ["sendEmail", "sendSms"] // <- "Yup, si cambia uno, actualiza el otro sin trabarte"
]);