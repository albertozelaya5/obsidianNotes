Dependencia `react-signature-canvas` `^1.1.0-alpha.2` agregada al proyecto (firma digital en creación de clientes).

> [!IMPORTANT]
> - feat/react-signature-canvas
> - feat/react-signature-canvas-dev
> - feat/react-signature-canvas-qa

- Un commit por rama, cada una desde su base (`origin/master`, `origin/dev`, `origin/qa`). Sin PRs.
- Commit: `chore: add react-signature-canvas dependency`
- master y dev: `package.json` + `package-lock.json` (solo entradas nuevas: `react-signature-canvas`, `signature_pad`, `@types/signature_pad`, `trim-canvas`).
- qa: **solo `package.json`** — la rama `qa` no versiona `package-lock.json` (se eliminó en el commit `4bc8a8d2`).
- La rama `feat/client-product-accounts` ya tenía la línea en su `package.json`; no se tocó.
