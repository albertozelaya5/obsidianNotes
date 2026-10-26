#aurora #git

Revisión del 2026-10-20. Cada rama vive en tres variantes: base (`origin/master`), `-dev` (`origin/dev`) y `-qa` (`origin/qa`). Ver [[Git Workflow Aurora]].

✅ = ya está en esa rama remota · ❌ = falta mergear · ⚠️ = no existe la variante

---

### Pendientes de mergear a master

| Rama                           | master        | dev       | qa        | Nota                                                        | Doc                                                    |
| ------------------------------ | ------------- | --------- | --------- | ----------------------------------------------------------- | ------------------------------------------------------ |
| `feat/claude-skills`           | ✅             | ✅         | ✅         | Skills `nueva-pantalla` y `nueva-rama`. Subidas, faltan PRs |                                                        |
| `fix/comparer-core-req-341`    | ❌ 123 commits | ✅         | ✅         | Rama larga, revisar bien antes del PR                       | [[SIPAD 341 - RICCY CAMBIOS FECHAS(VIDEO) Y BANCO]]    |
| `feat/prettier-config`         | ❌ 1 commit    | ✅ PR 1161 | ✅ PR 1162 |                                                             | [[Prettier config]]                                    |
| `fix/customer-quit-number`     | ❌ 1 commit    | ✅         | ✅         |                                                             | [[Quitar +504]]                                        |
| `feat/payroll-generator`       | ❌ 11 commits  | ✅         | ✅         |                                                             | [[Generador de planilla]], [[Generación de Plantilla]] |
| `feat/client-product-accounts` | ❌ 9 commits   | ✅         | ✅         |                                                             | [[Onboarding Creación de Clientes]]                    |
| `feat/accounting-safety`       | ❌ 1 commit    | ⚠️        | ⚠️        | Falta crear `-dev` y `-qa`                                  | [[Contabilidad - Tasa seguridad - Diego]]              |
|                                |               |           |           |                                                             |                                                        |

### Ya en master

- `feat/bank-remove-update-button`

### Ignorar (respaldos)

- `backup/pre-reorder`
- `backup/before-undo-rebase-688a943`

---

### To do

- [x] Subir `feat/claude-skills` (+ `-dev` / `-qa`)
- [ ] Abrir PRs de `feat/claude-skills` a master, dev y qa
- [x] Subir los commits pendientes de `fix/comparer-core-req-341`
- [ ] Crear `feat/accounting-safety-dev` y `-qa`
- [ ] PRs a master: prettier-config, customer-quit-number, payroll-generator, client-product-accounts, accounting-safety, comparer-core-req-341
- [ ] Borrar ramas `backup/*` cuando ya no hagan falta
