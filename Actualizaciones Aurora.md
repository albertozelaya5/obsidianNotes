
Este es un excelente plan de ataque. Al haber quitado los símbolos (`^` y `~`), **sí sigue siendo necesario usar `--save-exact` (o `-E`)** cuando instales algo nuevo. ¿Por qué? Porque si no lo pones, `npm` volverá a poner el símbolo automáticamente en tu `package.json`.

Aquí tienes el orden estratégico para actualizar. He dividido la lista en **"Oleadas"** para que el riesgo sea controlado:

---

### 🟢 Oleada 1: Utilidades y Herramientas (Riesgo Bajo)

Estas librerías rara vez rompen la aplicación principal. Son un buen lugar para empezar.

1. **`axios`** (Ya estás en la 1.16.1, mantén esa que es la segura). -
2. **`react-toastify`** (v10 → v11). -
3. **`react-timer-hook`** (v3 → v4). -
4. **`globals`** y **`@eslint/js`** (Herramientas de desarrollo). -

### 🟡 Oleada 2: UI y Formularios (Riesgo Medio)

Aquí debes probar que los formularios sigan validando y que los estilos no se rompan.

1. **`react-hook-form`** (Ya estás en la 7.75.0, manténla). -
2. **`@hookform/resolvers`** (v3 → v5). *Ojo: Revisa si cambió la forma de importar el resolver de Yup.* -
3. **`framer-motion`** (v11 → v12). *Suelen cambiar nombres de props de animación.* -
4. **`react-datepicker`** (v7 → v9). -
5. **`react-markdown`** (v9 → v10). -

### 🟠 Oleada 3: Estado Global y Datos (Riesgo Alto)

Aquí es donde vive el "corazón" de los datos de Banhcafe.

1. **`@reduxjs/toolkit`** y **`react-redux`** (Ya están en la versión *Wanted*, mantenlas ahí). -
2. **`styled-components`** (v5 → v6). *Cambio importante en cómo manejan el StyleSheetManager.* -
3. **`yup`** (0.32 → 1.7). **¡CUIDADO!** Este es un salto gigante. Yup cambió casi toda su API de la v0.32 a la v1.x. Tendrás que actualizar muchos esquemas de validación. => AQUI ME FALTA ACTUALIZAR

### 🔴 Oleada 4: El Core del Proyecto (Riesgo Crítico)

**Deja esto para un día que tengas mucho café.** No los actualices por separado; estos deben ir juntos.

1. **`vite`** (v6 → v8).
2. **`react`** y **`react-dom`** (v18 → v19).
3. **`react-router-dom`** (v6 → v7). *La v7 es una fusión con Remix; lee la guía de migración.*
4. **`typescript`** (v5.9 → v6.0).

---

### 🛠️ ¿Cómo hacerlo con "Save Exact"?

Cada vez que decidas actualizar una, usa el comando con `-E`:

```bash
# Ejemplo para Yup (que es urgente por seguridad pero difícil por código)
npm install yup@latest -E
```

### 💡 Mi consejo profesional para Banhcafe:

**No actualices `devextreme` todavía.**
Veo que `devextreme` está en la **24.2.14** y la última es la **25.2.7**. DevExtreme es una librería de componentes empresariales pesada. Actualizarla puede romper tablas (DataGrids) o reportes enteros. Si los componentes actuales te funcionan, quédate en la v24 hasta que tengas tiempo de probar cada tabla del sistema.

### librerias no usadas posibles

- `react toaster`
- `axios` => se puede cambiar lo que esta ahorita por rtk query - casi no se usa
- `react-timer-hook` => no es tan popular y es inactivo, se recomienda cambiar por otra libreria
- `react-datepicker` => tiene poca frecuencia de actualizaciones
- `react-markdown` => tiene poca frecuencia de actualizaciones - casi no se usa
- `yup` => casi no tiene comunidad y hace 8 meses no se actualizan

## Correcciones

- styleTable en una etiqueta html

medios de pado > central de riesgos > Equifax

Timeline
CommentBox
DragDrop

---
DateBox => useCustomInputError

5.3.11 styled components

