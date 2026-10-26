---
tags:
  - mia
  - pendientes
  - git
---

> [!info] Estado al 2026-10-19
> `dev` y `qa` ya tienen el fix del login y el de zod v4 / recursos (PRs 1144, 1146, 1148, 1149). El código de `dev` y `qa` es idéntico.
> Falta pasar `qa` → `master` (producción). Ahora me encargo yo de los pases, ya no Carlos.

## 1. Pasar `qa` → `master` (producción)

**Conflictos:** ninguno. `master` no tiene commits propios (está 61 commits atrás de `qa`), así que solo avanza hasta `qa`.

> [!warning] Es un salto grande
> A producción se van 61 commits y 144 archivos acumulados desde septiembre de 2025, no solo mis cambios.
>
> | | `master` hoy | `qa` |
> |---|---|---|
> | Next.js | 15.2.3 | 16.2.6 |
> | DevExtreme | 24.2.5 | 25.2.7 |
> | zod | 3.24 | 4.4.3 |
> | TypeScript | 5.6 | 6.0 |
> | Instalación / Dockerfile | yarn | pnpm |

### Checklist

- [ ] Probar QA a fondo: login, grupos, roles, políticas, servicios, usuarios, atributos
- [ ] Averiguar dónde se hace el deploy de producción y de dev (**no** está en Pipelines del proyecto MIA en Azure DevOps)
- [ ] **yarn → pnpm:** el `Dockerfile` de `qa` ya usa pnpm (`pnpm install --frozen-lockfile` y `pnpm run build`)
	- Si el deploy usa ese Dockerfile → no hay que cambiar comandos
	- Si algún script en el servidor corre `yarn install` / `yarn build` → cambiarlo a `pnpm install` / `pnpm run build`
- [ ] **Variables del build:** como son `NEXT_PUBLIC_`, Next las mete en el JS **al compilar**. Tienen que llegar como `--build-arg` al `docker build`, no como variables del contenedor:
	- [ ] `NEXT_PUBLIC_API_BASE_URL` (la de producción)
	- [ ] `NEXT_PUBLIC_API_KEY` (la de producción)
	- [ ] `NEXT_PUBLIC_NODE_ENV=production`
	- [ ] `NEXT_PUBLIC_DEVEXTREME_KEY` → la clave `ewog…` del `.env` (cubre hasta la 25.2). **Esta es la nueva a revisar**
- [ ] Anotar el commit actual de `master` para poder regresar: **`d600679`**
- [ ] Hacer PR `qa` → `master`
- [ ] Después del deploy, revisar en producción:
	- [ ] Que haga login
	- [ ] Que **no** salga el banner "For evaluation purposes only" de DevExtreme
	- [ ] Que no haya errores en la consola (F12)

> [!tip] Si sale el banner de DevExtreme
> Busca en la consola el código de la advertencia:
> - **W0019** → no llegó ninguna clave al build (falta el `--build-arg`)
> - **W0020** → la clave es para una versión anterior
> - **W0021** → la clave es inválida o es la `LCX…`. La `LCX` **no** se usa directo, hay que usar la `ewog…`
>
> Después de corregir la variable hay que **volver a hacer el build**. Reiniciar el contenedor no sirve.

## 2. Arreglar también el deploy de **dev**

- [ ] En el servidor de dev sale el banner de prueba de DevExtreme. El código y la clave están bien (en local con la 25.2.7 no sale), así que el problema es que el build de dev no recibe `NEXT_PUBLIC_DEVEXTREME_KEY`. Hay que revisar lo mismo de la sección 1.

## 3. Flujo de ramas

Según el `README.md`:

```
master ──► mi rama ──PR──► dev ──PR──► qa ──PR──► master
```

- `master` → código en producción
- `qa` → donde se prueba antes de producción
- `dev` → donde se integran las ramas

**Nombres de rama:** `fix-nombre`, `feat-nombre`, `refactor-nombre` (o `fix-req-123` si hay requerimiento).

### Paso a paso

1. **Crear la rama**
	- Si `master` ya está al día (después del paso `qa` → `master`):
	  ```bash
	  git fetch
	  git switch -c fix-nombre origin/master
	  ```
	- Si `master` vuelve a quedarse atrasada → crearla desde `origin/dev`, para no trabajar sobre código viejo y evitar conflictos
2. **Trabajar y subir**
	```bash
	git add <archivos>
	git commit -m "fix: descripción"
	git push -u origin fix-nombre
	```
	⚠️ No subir `pnpm-workspace.yaml`: se genera localmente con `pnpm install`.
3. **PR rama → `dev`**: Azure DevOps → Repos → Pull requests → New pull request
4. **Pasar a `qa`**, con una de estas dos opciones:
	- PR `dev` → `qa` → sigue el flujo, pero se lleva **todo** lo que haya en `dev`, también cosas de otros que quizá no están listas
	- PR mi rama → `qa` → solo pasa mi cambio (lo más seguro si en `dev` hay cosas a medias)
5. **PR `qa` → `master`**: cuando QA lo apruebe

### Si un PR marca conflicto

```bash
git fetch
git switch fix-nombre
git merge origin/qa          # o origin/dev, según el destino
# resolver los archivos en conflicto
git add <archivos>
git commit
git push
```

> [!note] PR con "Files 0"
> Si al abrir un PR sale **Files 0**, la rama destino ya tiene todos esos cambios (solo faltaría historial, como un commit de merge). No hace falta crear ese PR.

### Antes de subir, comprobar que compila

```bash
pnpm install
pnpm run build
pnpm start          # el "preview" de Next
```

Al cambiar de rama, correr `pnpm install` si cambió el `package.json`: cambiar de rama **no** reinstala las dependencias.

## 4. Limpieza

- [ ] Borrar en Azure DevOps las ramas `fix-zod-v4-build` y `fix-changes-login-key` (ya están en `dev` y `qa`)
- [ ] Borrar la rama local de respaldo: `git branch -D backup/fix-changes-login-key-master`
- [ ] Comentar al equipo: `tsconfig.json` todavía tiene `target: es5` y `baseUrl`, que dejan de funcionar en TypeScript 7 (hoy no rompen el build)
- [ ] ESLint del repo está roto (error de config `Converting circular structure to JSON`)
