
| **Acción**               | **Comando npm**                                                                                          | **Comando pnpm**                                           |
| ------------------------ | -------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| Instalar todo            | `npm install`                                                                                            | `pnpm i`                                                   |
| Añadir paquete           | `npm install <pkg>`                                                                                      | `pnpm add <pkg>`                                           |
| Añadir a devDependencies | `npm install <pkg> -D`                                                                                   | `pnpm add -D <pkg>`                                        |
| Eliminar paquete         | `npm uninstall <pkg>`                                                                                    | `pnpm remove <pkg>`                                        |
| Ejecutar scripts         | `npm run <script>`                                                                                       | `pnpm <script>` (o `pnpm run`)                             |
| Aprovar ejecucion        |                                                                                                          | `pnpm approve-builds`                                      |
| Correr a prod            | `npm run build`                                                                                          | `pnpm build`                                               |
| Preview                  | `npm run preview`                                                                                        | `pnpm preview`                                             |
| Package lock json        |                                                                                                          | `pnpm import`, luedo del package log json                  |
| Para actualizar          | `npm outdated`<br>`npm update`<br>`npx npm-check-updates`<br>`npx npm-check-updates -u`<br>`npm install` | `pnpm outdated`<br>`pnpm update`<br>`pnpm update --latest` |
| Limpiar cache            | `npm cache clean --force`                                                                                | `pnpm store prune`                                         |
`pnpm update --interactive`=> para elegir que actualizar visualmente
