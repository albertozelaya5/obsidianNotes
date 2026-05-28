```powershell
git reset --hard HEAD
```

- Apunta al último commit de la rama en la que estás trabajando (commit local)
- Si haces un nuevo commit, el `HEAD` se mueve automáticamente a ese nuevo commit

```powershell
git reset --hard HEAD~1
```

Representa al **padre del commit actual**. Es decir, el commit exactamente anterior al que tienes seleccionado ahora.

Meteora27182