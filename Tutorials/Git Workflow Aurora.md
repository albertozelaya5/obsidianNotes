
## **Objetivo**

Este proyecto utiliza un flujo de ramas que permite mantener separados los ambientes de **Desarrollo**, **QA**, **Pre-Producción** y **Producción**, evitando que funcionalidades en desarrollo lleguen accidentalmente a producción.

Cada ambiente puede contener diferentes requerimientos, por lo que **nunca se realizan merges directos entre ramas principales** (`dev`, `qa`, `pre-production` y `master`) para promover funcionalidades individuales.

La promoción entre ambientes se realiza mediante **Cherry Pick** y **Pull Requests**.

---

# **Estructura de ramas**

|**Rama**|**Descripción**|
|---|---|
|`master`|Código actualmente en Producción|
|`pre-production`|Código listo para salir a Producción|
|`qa`|Código en pruebas de QA|
|`dev`|Código en integración y desarrollo|

---

# **Filosofía del flujo**

Cada requerimiento tiene una única rama principal de desarrollo.

```text
master
   │
   ▼
feat/REQ-1500
```

Esta rama es la **fuente de verdad** del requerimiento.

Desde ella se promocionan los cambios hacia cada ambiente.

---

# **Flujo completo**

```text
                         master
                            │
                            ▼
                     feat/REQ-1500
                            │
            ┌───────────────┼────────────────┐
            │               │                │
            ▼               ▼                ▼
    feat/REQ-1500-dev  feat/REQ-1500-qa      │
            │               │                │
            ▼               ▼                ▼
         PR → dev        PR → qa     PR → pre-production
                                                 │
                                                 ▼
                                            PR → master
```

---

# **Paso 1 - Crear una nueva funcionalidad**

Siempre iniciar desde `master`.

```bash
git checkout master
git pull origin master

git checkout -b feat/REQ-1500
```

Trabajar normalmente sobre esta rama.

```bash
git add .
git commit -m "feat: add customer search"
```

Todos los cambios del requerimiento permanecen en esta rama.

---

# **Paso 2 - Promover a Desarrollo**

Actualizar `dev`.

```bash
git checkout dev
git pull origin dev
```

Crear una rama temporal desde `dev`.

```bash
git checkout -b feat/REQ-1500-dev
```

Copiar únicamente los commits del requerimiento.

```bash
git cherry-pick <SHA>
```

o varios commits.

```bash
git cherry-pick SHA1 SHA2 SHA3
```

Publicar la rama.

```bash
git push origin feat/REQ-1500-dev
```

Crear Pull Request.

```text
feat/REQ-1500-dev
        │
        ▼
       dev
```

Eliminar la rama temporal una vez finalizado el Pull Request.

---

# **Paso 3 - Promover a QA**

Actualizar la rama.

```bash
git checkout qa
git pull origin qa
```

Crear una rama temporal.

```bash
git checkout -b feat/REQ-1500-qa
```

Aplicar Cherry Pick.

```bash
git cherry-pick <SHA>
```

Publicar la rama.

```bash
git push origin feat/REQ-1500-qa
```

Crear Pull Request.

```text
feat/REQ-1500-qa
        │
        ▼
        qa
```

Eliminar la rama temporal al finalizar.

---

# **Paso 4 - Promover a Pre-Producción**

Para este ambiente **no se crea una rama temporal**.

Se utiliza la rama original del requerimiento.

```text
feat/REQ-1500
        │
        ▼
PR → pre-production
```

Una vez aprobado el Pull Request, la funcionalidad queda integrada en `pre-production`.

---

# **Paso 5 - Promover a Producción**

Cuando todas las funcionalidades aprobadas ya se encuentran en `pre-production`, se crea un Pull Request desde esta rama hacia `master`.

```text
pre-production
        │
        ▼
      master
```

No se realiza Cherry Pick para Producción.

No se crea una rama temporal desde `pre-production`.

La rama `pre-production` es la única fuente para promover cambios hacia Producción.

---

# **Resumen del flujo**

```text
1. Crear feature

master
   │
   ▼
feat/REQ-1500


2. Desarrollo

dev
 │
 ▼
feat/REQ-1500-dev
 │
 ▼
PR → dev


3. QA

qa
 │
 ▼
feat/REQ-1500-qa
 │
 ▼
PR → qa


4. Pre-Producción

feat/REQ-1500
      │
      ▼
PR → pre-production


5. Producción

pre-production
      │
      ▼
master
```

---

# **¿Por qué utilizar Cherry Pick?**

Cada ambiente puede contener diferentes requerimientos.

Ejemplo:

**master**

```text
REQ-100
REQ-101
REQ-102
```

**dev**

```text
REQ-100
REQ-101
REQ-102
REQ-103
REQ-104
REQ-105
```

Si únicamente el requerimiento **REQ-104** debe llegar a QA, realizar un Merge desde `dev` también llevaría los requerimientos 103 y 105.

Con Cherry Pick únicamente se promueven los commits necesarios.

---

# **Buenas prácticas**

- Crear siempre las funcionalidades desde `master`.
- Mantener la rama `feat/*` como la rama principal del requerimiento.
- No desarrollar directamente sobre `dev`, `qa`, `pre-production` o `master`.
- Utilizar ramas temporales únicamente para `dev` y `qa`.
- Eliminar las ramas temporales una vez finalizado el Pull Request.
- Mantener commits pequeños y descriptivos.
- Preferir un único commit por requerimiento o realizar Squash antes de promocionar los cambios.

---

# **Ejemplo completo**

```bash
# Crear feature
git checkout master
git pull origin master
git checkout -b feat/REQ-1500

# Desarrollo
git add .
git commit -m "feat: add customer search"

# Promover a dev
git checkout dev
git pull origin dev
git checkout -b feat/REQ-1500-dev
git cherry-pick <SHA>
git push origin feat/REQ-1500-dev

# Promover a QA
git checkout qa
git pull origin qa
git checkout -b feat/REQ-1500-qa
git cherry-pick <SHA>
git push origin feat/REQ-1500-qa

# Promover a Pre-Producción
git checkout feat/REQ-1500
git push origin feat/REQ-1500

# Crear PR:
# feat/REQ-1500 -> pre-production

# Crear PR:
# pre-production -> master
```