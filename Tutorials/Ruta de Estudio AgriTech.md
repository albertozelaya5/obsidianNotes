Orden recomendado para entender el stack de **AgriTech WebSite** (TanStack Start + Router + Query + shadcn/ui + Radix) en el menor tiempo posible, partiendo de que ya sabés React. **Total estimado: ~2–3 días de estudio + 1 fin de semana de práctica.**

Notas involucradas: [[Shadcn UI]] · [[React Query (Tanstack query)]] · [[TanStack Router y Start]]

> [!tip] Regla de oro
> Cada paso termina **leyendo un archivo real de AgriTech**. Si entendés ese archivo, pasá al siguiente paso; si no, volvé a la sección de la nota.

---

## Paso 1 — shadcn/ui + Radix (≈ 2–3 h)

📖 [[Shadcn UI]]: secciones 1–9 (qué es, `components.json`, tokens, `cva`, `cn()`, `asChild`, editar in-place).

- Saltá por ahora: 10 (Form + RHF), 14 (Charts). AgriTech **no** usa React Hook Form en el LeadForm.
- Radix en 3 ideas: composición por partes (`Dialog` / `DialogTrigger` / `DialogContent`), `asChild`, atributos `data-state`.

🔍 Leé en AgriTech:
- [ ] `src/components/ui/button.tsx` (cva + variantes)
- [ ] `src/components/ui/dialog.tsx` (Radix envuelto)
- [ ] `src/lib/utils.ts` (`cn`)
- [ ] `src/styles.css` (tokens: `bg-primary`, `text-foreground`, escala `text-14`…)

---

## Paso 2 — React Query (≈ 2–3 h)

📖 [[React Query (Tanstack query)]]: secciones 1–9 (server state, `useQuery`, keys, `staleTime`, `useMutation`, `invalidateQueries`).

- Lo vas a necesitar **antes** del Router porque el paso 4 los une.

🔍 Leé en AgriTech:
- [ ] `src/router.tsx` (dónde nace el `QueryClient`)

---

## Paso 3 — TanStack Router (≈ 3–4 h)

📖 [[TanStack Router y Start]]: secciones 1–9.

Orden interno sugerido: 2 (nombres de archivo) → 3 (`createFileRoute`) → 4 (`<Link>`) → 5 (params) → 7 (loader) → 6 (search params) → 8 (layouts) → 9 (guards).

🔍 Leé en AgriTech:
- [ ] `src/routes/README.md`
- [ ] `src/routes/index.tsx` (ojo: los `<Hero />;` sueltos son código muerto; lo que se renderiza está en `LandingPage()`)
- [ ] `src/routes/blog.$slug.tsx` (param + loader + `notFound`: **el archivo más didáctico del repo**)
- [ ] `src/components/layout/Navbar.tsx` (links `to` vs `hash`)

---

## Paso 4 — Router + Query juntos (≈ 1 h)

📖 [[TanStack Router y Start]]: sección 10 (`queryOptions` → `ensureQueryData` → `useSuspenseQuery`).

🔍 Leé en AgriTech:
- [ ] `src/routes/__root.tsx`: `createRootRouteWithContext<{ queryClient }>`

---

## Paso 5 — TanStack Start (≈ 1–2 h, sin profundizar)

📖 [[TanStack Router y Start]]: secciones 11–14.

> [!warning]
> AgriTech migra a **Astro**: esto es lo que menos se reutiliza. Entendelo, no lo domines.

🔍 Leé en AgriTech:
- [ ] `src/routes/__root.tsx` completo (`head`, `shellComponent`, `HeadContent`, `Scripts`)
- [ ] `src/server.ts` (solo por encima: manejo de errores SSR custom)

---

## Paso 6 — Repaso (≈ 30 min)

- [ ] Secciones "Errores comunes" y "Checklist" de las **tres** notas.

---

## Paso 7 — Practicar: "Mercado de Cosecha" (1 fin de semana)

📖 [[TanStack Router y Start]]: sección 17.

- [ ] Feature 1 — Lista (server fn + loader + Query)
- [ ] Feature 2 — Filtros en la URL
- [ ] Feature 3 — Detalle + `notFound` + skeleton
- [ ] Feature 4 — Publicar lote (`Dialog` + `useMutation`)
- [ ] Feature 5 — Ofertar (optimistic update)
- [ ] Feature 6 — `/api/lotes`
- [ ] Feature 7 — Bonus: `beforeLoad` + `redirect`

---

## Paso 8 — Volver a AgriTech

Primer cambio real para fijar todo: **una ruta nueva + un `Dialog` de shadcn**, usando tokens de `styles.css` (nada de hex).

- [ ] Ruta nueva en `src/routes/` que compone `<NavBar />` + sección + `<Footer />`
- [ ] Sección en `src/components/sections/` (carpeta propia si crece, como `calculator/`)
- [ ] `bun run lint` sin errores nuevos

---

## Resumen

| # | Qué | Nota | Tiempo |
| --- | --- | --- | --- |
| 1 | shadcn + Radix | [[Shadcn UI]] §1–9 | 2–3 h |
| 2 | React Query | [[React Query (Tanstack query)]] §1–9 | 2–3 h |
| 3 | Router | [[TanStack Router y Start]] §1–9 | 3–4 h |
| 4 | Router + Query | [[TanStack Router y Start]] §10 | 1 h |
| 5 | Start | [[TanStack Router y Start]] §11–14 | 1–2 h |
| 6 | Repaso | las tres | 30 min |
| 7 | Mini proyecto | [[TanStack Router y Start]] §17 | fin de semana |
| 8 | Cambio real en AgriTech | — | 2–4 h |
