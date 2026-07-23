
| Comando                                              | Descripción                                                                                                                                |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `git fetch`                                          | Descarga los cambios nuevos desde el servidor (Azure, GitHub, etc.) a tu base de datos local, pero **no toca tu código** todavía.          |
| `git merge`                                          | Toma los cambios que ya están descargados en tu máquina y los **fusiona** con la rama en la que estás parado actualmente.                  |
| `git merge --abort`                                  | Para abortar un merge con conflictos.                                                                                                      |
| `git merge origin/master`                            | Trae los cambios remotos y los fusiona en el historial mediante un commit unión. Luego se pone `shift o`, un mensaje y luego `esc` y `:x`. |
| `git rebase master`                                  | Mueve el historial de la rama master hacia atrás, y mueve los cambios de mi rama local hasta después de los cambios de master.             |
| `git revert HEAD`                                    | Hace exactamente lo opuesto al commit que se quiere borrar.                                                                                |
| `git diff --name-only master...fix/swift-req-590-v2` | Lista solo los nombres de archivos que difieren entre `master` y la rama indicada.                                                         |
| `git rebase -X theirs master`                        | Rebase contra master, resolviendo automáticamente los conflictos a favor de los cambios de master (`theirs`).                              |
| `git restore archivo.txt`                            | Descarta los cambios no commiteados de un archivo modificado (que no está en staging).                                                     |
| `git restore --staged --worktree archivo.txt`        | Descarta los cambios de un archivo que ya está en staging (con `git add`) pero sin commit.                                                 |
| `git restore .`                                      | ⚠️ Descarta TODOS los cambios no commiteados del repo. Revisa `git status` antes de correrlo, es destructivo.                              |
| git push origin --all                                | Esto sube todas tus ramas locales a origin                                                                                                 |

2. Sin tocar el remoto: usar un bundle
git bundle create repo.bundle --all
Esto empaqueta todo (incluidas ramas no subidas) en un archivo. Luego en el otro lugar:
git clone repo.bundle nuevo-repo

3. Clonar directamente desde tu copia local
git clone --branch <rama> /ruta/al/repo/actual nuevo-repo
# o para traer todas:
git clone /ruta/al/repo/actual nuevo-repo
cd nuevo-repo
git branch -a   # verlas
Git trata el repo local como si fuera un remoto más, así que todas tus ramas (incluso sin push) quedan disponibles como origin/rama en el clon.

¿Cuál es tu caso: quieres moverlas a otra máquina, o simplemente respaldarlas sin subirlas a origin?