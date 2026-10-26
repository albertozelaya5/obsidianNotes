# Comandos Git más usados

## Preparar cambios (staging)

| Comando | Qué hace |
|---|---|
| `git status` | Muestra qué archivos cambiaron, cuáles están en staging y cuáles no. |
| `git add -A` | Agrega **todo** al staging (nuevos, modificados y borrados) en **todo el repo**, sin importar en qué carpeta estés. |
| `git add .` | Agrega todo, pero solo desde la **carpeta actual hacia abajo**. Si estás en la raíz, es igual que `-A`. |
| `git add -p` | Agrega por partes (hunks), preguntándote uno por uno. Útil para separar cambios en varios commits. |
| `git diff` | Cambios que **no** están en staging. |
| `git diff --staged` | Cambios que **sí** están en staging (lo que entra en el próximo commit). |

## Commits

| Comando | Qué hace |
|---|---|
| `git commit -m "msg"` | Crea un commit con lo que está en staging. |
| `git commit --amend` | Modifica el último commit (mensaje o contenido). No usar si ya hiciste push (o usar luego `push --force-with-lease`). |
| `git log --oneline --graph` | Historial compacto con el árbol de ramas. |
| `git show <hash>` | Muestra el contenido de un commit. |

## Deshacer cosas

| Comando | Qué hace |
|---|---|
| `git restore <archivo>` | Descarta los cambios **no staged** de un archivo (vuelve a como estaba en el último commit). ⚠️ Se pierden. |
| `git restore --staged <archivo>` | Saca el archivo del staging, pero **conserva** los cambios. (El "des-add".) |
| `git restore --source=<rama/hash> <archivo>` | Trae la versión de un archivo desde otra rama o commit. |
| `git revert HEAD` | Crea un **nuevo commit** que deshace el último. Seguro para ramas ya subidas (no reescribe historia). `git revert <hash>` para uno específico. |
| `git reset --soft HEAD~1` | Deshace el último commit pero deja los cambios en staging. |
| `git reset HEAD~1` | Igual, pero deja los cambios fuera del staging (`--mixed`, el default). |
| `git reset --hard HEAD~1` | Borra el último commit **y** sus cambios. ⚠️ Destructivo. |
| `git reset --hard origin/<rama>` | Deja tu rama idéntica a la remota, descartando todo lo local. ⚠️ |
| `git reflog` | Historial de todo lo que hizo HEAD. Salvavidas para recuperar commits "perdidos" tras un reset/rebase. |

> `revert` = deshacer de forma segura (nuevo commit). `reset` = reescribir historia (solo en ramas locales o propias).

## Stash (guardar cambios temporalmente)

| Comando | Qué hace |
|---|---|
| `git stash` | Guarda los cambios sin commitear y deja el working tree limpio. |
| `git stash push -m "mensaje"` | Igual, con un nombre para identificarlo. |
| `git stash -u` | Incluye también archivos nuevos (untracked). |
| `git stash list` | Lista los stashes (`stash@{0}` es el más reciente). |
| `git stash show -p stash@{0}` | Muestra el diff de un stash. |
| `git stash apply stash@{0}` | Aplica el stash pero **lo conserva** en la lista. |
| `git stash pop` | Aplica el último stash y **lo borra** de la lista. |
| `git stash drop stash@{0}` | Borra un stash. |

## Ramas

| Comando | Qué hace |
|---|---|
| `git branch` / `git branch -a` | Lista ramas locales / locales + remotas. |
| `git checkout -b <rama> origin/master` | Crea una rama nueva basada en `origin/master` y se cambia a ella. |
| `git switch <rama>` | Cambia de rama (versión moderna de `checkout`). `git switch -c <rama>` para crear. |
| `git branch -d <rama>` | Borra una rama local ya mergeada (`-D` para forzar). |
| `git push origin --delete <rama>` | Borra la rama en el remoto. |

## Remoto

| Comando | Qué hace |
|---|---|
| `git fetch origin` | Descarga cambios del remoto **sin** tocar tus ramas. |
| `git pull` | `fetch` + `merge` de la rama remota en la actual. `git pull --rebase` para hacer rebase en lugar de merge. |
| `git push -u origin <rama>` | Sube la rama y la deja vinculada (después basta `git push`). |
| `git push --force-with-lease` | Push forzado, pero falla si alguien más subió algo que no tienes. Más seguro que `--force`. |
| `git remote -v` | Muestra las URLs de los remotos. |
| `git remote set-url origin <url>` | Cambia la URL del remoto (p. ej. pasar de HTTPS a SSH, o mover el repo). |
| `git remote add <nombre> <url>` | Agrega otro remoto. |

## Integrar cambios

| Comando | Qué hace |
|---|---|
| `git merge <rama>` | Une otra rama en la actual (crea un merge commit si hace falta). |
| `git rebase origin/master` | Reaplica tus commits encima de `origin/master`. Historia lineal. ⚠️ Reescribe commits → luego `push --force-with-lease`. |
| `git rebase -i HEAD~3` | Rebase interactivo: reordenar, juntar (squash), editar o borrar los últimos 3 commits. |
| `git rebase --continue` / `--abort` | Continuar tras resolver conflictos / cancelar el rebase. |
| `git cherry-pick <hash>` | Copia un commit específico a la rama actual. (Así paso commits a las ramas `-dev` y `-qa`.) |
| `git cherry-pick --continue` / `--abort` | Igual que en rebase. |

## Flujo usado en BANHCAFE (rama + `-dev` + `-qa`)

```bash
git fetch origin
git checkout -b fix/mi-cambio origin/master
# ...cambios...
git add -A && git commit -m "fix: ..."
git checkout -b fix/mi-cambio-dev origin/dev && git cherry-pick fix/mi-cambio
git checkout -b fix/mi-cambio-qa  origin/qa  && git cherry-pick fix/mi-cambio
git push -u origin fix/mi-cambio fix/mi-cambio-dev fix/mi-cambio-qa
```
