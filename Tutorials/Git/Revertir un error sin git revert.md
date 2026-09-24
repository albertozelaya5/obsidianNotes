Cuando eso pasa, se debe hacer una rama a partir de la afectada, se le hace un git revert a esa rama, y luego se hace un pr

## Procedimiento: revertir un cambio ya mergeado

### 1. Rama temporal desde la rama afectada

```bash
```
git checkout master && git pull
git checkout -b fix/revertir-algo


2. Revert del commit malo

git revert <hash_malo>

Si <hash_malo> es un merge commit (tipo "Merged PR ###"), hay que indicar el mainline:
git revert -m 1 <hash_malo>
(-m 1 = mantener el lado de la rama destino, deshacer lo que trajo la rama mergeada)

3. Push y PR de vuelta a la rama afectada

git push -u origin fix/revertir-algo
→ abrir PR en Azure DevOps hacia master (o la rama correspondiente)

4. Replicar en dev/qa (si el bug llegó a las 3 ramas por cherry-pick)

En vez de repetir git revert en cada rama, cherry-pickear el commit de revert ya probado:
git checkout dev && git pull
git cherry-pick <hash_del_revert>

git checkout qa && git pull
git cherry-pick <hash_del_revert>

---

Plan B: si git revert da conflictos irresolubles

1. Rama temporal

git checkout -b temp-revert-algo

2. Generar el diff invertido (nuevo → viejo)

git diff <hash_malo> <hash_malo>^ > /tmp/reverso.patch

3. Aplicar el parche y commitear

git apply /tmp/reverso.patch
git add -A
git commit -m "fix: revertir <descripción del problema>"