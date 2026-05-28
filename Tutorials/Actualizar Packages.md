
Hay varias formas, pero esta automatiza y brinda diferentes opciones para actualizar a la patch, minor, latest, newest, greatest

```
npm install -g npm-check-updates
ncu --target latest
ncu -u
```

1. `--target patch` (El más seguro)

- **Qué hace:** Solo busca actualizaciones en el tercer dígito (ej. de `1.0.1` a `1.1.5`).
    
- **Seguridad:** **Máxima**. Estas versiones solo incluyen correcciones de errores (bug fixes) y parches de seguridad.
    
- **Riesgo:** Casi nulo de que rompa tu código.
    

2. `--target minor` (Equilibrio ideal)

- **Qué hace:** Actualiza el segundo dígito (ej. de `1.2.0` a `1.8.0`).
    
- **Seguridad:** **Alta**. Según las reglas de SEMVER, las versiones "minor" añaden funcionalidades nuevas pero **no eliminan** ni rompen lo anterior.
    
- **Riesgo:** Bajo. Es la mejor opción si quieres nuevas capacidades sin reescribir código.
    

3. `--target latest` (El estándar)

- **Qué hace:** Actualiza a la versión más reciente que el autor de la librería ha marcado como "estable" en el registro de NPM. Puede saltar de la `v1` a la `v2` (Major updates).
    
- **Seguridad:** **Media**. Si hay un cambio de versión mayor (ej. de React 18 a 19), es muy probable que haya "Breaking Changes" (cambios que rompen cosas).
    
- **Riesgo:** Moderado. Es lo que usamos para llevar el proyecto a React 19.
    

 4. `--target newest`

- **Qué hace:** Busca la versión con la fecha de publicación más reciente, incluso si el número de versión es menor que la actual (algo muy raro, pero pasa si un autor lanza un parche para una versión vieja).
    
- **Uso:** Muy poco común en el desarrollo diario.
    

 5. `--target greatest`

- **Qué hace:** Simplemente busca el número de versión más alto posible disponible en el registro, ignorando etiquetas de "estable" o fechas.
    
- **Riesgo:** Alto, podrías terminar con versiones experimentales si no tienes cuidado.