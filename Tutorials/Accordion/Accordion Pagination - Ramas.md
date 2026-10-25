> [!IMPORTANT]
> - feat/accordion
> - feat/accordion-dev
> - feat/accordion-qa

## Qué

Props opcionales de paginación en `src/components/dev-extreme/Accordion.tsx`
(local + server-side). Todo opt-in: sin `paginated` el componente queda igual
que antes. Ver [[How to make an Accordion]].

## Ramas

| rama | base | commit |
|---|---|---|
| `feat/accordion` | `origin/master` | `53911ff4f` |
| `feat/accordion-dev` | `origin/dev` | `4f99a7249` |
| `feat/accordion-qa` | `origin/qa` | `2470a7643` |

- Mismo commit en las 3 (cherry-pick, aplicó limpio; `Accordion.tsx` era
  idéntico en master/dev/qa).
- Pusheadas. **Sin PR todavía.**
- Los commits NO llevan trailer de co-autoría de Claude.
- `tsc --noEmit` limpio.
- Nombre anterior: `feat/banking-param-1386{,-dev,-qa}` (renombradas a `feat/accordion*`, las viejas borradas de origin).

## Antecedente

Salió del trabajo en `feat/client-product-accounts` (`ModalViewProducts.tsx`
tenía la paginación local inline). Antes hubo un intento sólo-local en las ramas
`feat/accordion-local-pagination{,-dev,-qa}` — quedaron obsoletas por estas.
