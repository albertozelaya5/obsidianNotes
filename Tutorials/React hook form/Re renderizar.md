
- `shouldDirty`: marca el campo como "modificado" (isDirty = true). Sirve para saber si el usuario cambió algo (ej. mostrar un botón "guardar" o alertar si sale sin guardar).

- `shouldTouch`: marca el campo como "tocado" (isTouched = true). Se usa normalmente para mostrar errores de validación solo después de que el usuario interactuó con ese campo.

- `shouldValidate`: fuerza a que ese campo corra sus validaciones (yup en tu caso) inmediatamente después del setValue, y es lo que además garantiza que se dispare el re-render hacia los componentes que están viendo ese valor (useWatch/Controller).

### Primeros cambios

TransferContext.tsx
- Agregaste la prop isEdit: boolean al contexto para que los hijos sepan si están editando una transferencia existente.
- Sacaste resolvedLocalActorId de lo que expone el contexto (sigue existiendo dentro de useActorPool para isLocalAccount, solo dejó de exponerse hacia afuera). Verifiqué que ya nadie más lo consume desde el contexto — limpio, no rompe nada.

TransferForm.tsx
- Le pasas isEdit={isEdit} a <TransferContext>.
- El DateBox de "Fecha valor de liquidación" ahora solo se renderiza si !isTemplate (antes salía siempre).
- Los setValue de onGetProduct ya quedaron con shouldDirty/shouldTouch/shouldValidate (lo que arreglamos juntos).

ActorCard.tsx — el cambio más importante:
- El useEffect que ponía el nombre placeholder ("Nuevo Cliente..." / "Nueva Institución...") ahora tiene if (isEdit) return; — es decir, ya no pisa el nombre real cuando estás editando una transferencia existente, solo aplica el placeholder en transferencias nuevas. Este era justo el riesgo que yo había señalado antes, y lo corregiste bien.
- La alerta de "cuenta local" (LocalAlert) ya no depende de resolvedLocalActorId/getActorById — ahora se muestra con solo isDebtor, y lee account vía useWatch en vez de un objeto localActor fijo, así que se actualiza en tiempo real.
- El texto de esa alerta ahora decide "cuenta corriente/ahorros" vs "cuenta contable (GL)" según useGL (no pacs008), y siempre muestra la frase de que el backend resuelve los datos.

Un detalle a limpiar: en la línea 70 de ActorCard.tsx quedó un - suelto entre los dos useEffect. No rompe la compilación (JS lo interpreta como una negación unaria del segundo useEffect, que igual se ejecuta), pero es un residuo de edición que conviene quitar para que quede limpio. ¿Lo elimino?

