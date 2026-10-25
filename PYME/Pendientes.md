# Pendientes — BANHCAFE Pyme

Proyecto: `/Users/albertozelaya/Documents/Backup ALBERTO 2/banhcafe-projects/BANHCAFE-Pyme`

> Las líneas corresponden al código al 2026-09-18. Si editas los archivos se corren; para relocalizar los TODO usa `grep -rn "TODO" src`.

## 1. Login y sesión (`TODO(auth)`)

Hoy la sesión es simulada: el store arranca autenticado con un "Usuario Demo" y no existe la ruta `/login`.

- [ ] **Quitar la sesión simulada.** Iniciar con `accessToken: null`, `user: null`, `isAuthenticated: false`. Llamar `setSession` solo cuando la mutación de login tenga `isSuccess`; si falla, no guardar sesión ni navegar. Decidir si la sesión se persiste ("Recordarme").
  - `src/features/auth/store/authStore.ts:18` (comentario TODO)
  - `src/features/auth/store/authStore.ts:26-28` (valores simulados: token, usuario, `isAuthenticated: true`)
- [ ] **Crear la pantalla y la ruta `/login`.**
  - Ruta nueva en `src/app/router.tsx`, arriba de la rama `/` (la rama `/` empieza en `src/app/router.tsx:19`).
  - Con `loader` que redirija a `/` si ya hay sesión.
  - Página nueva `src/pages/LoginPage.tsx` y feature `src/features/auth/` (`api/`, `components/`, `schemas/`), usando `features/dashboard` como modelo.
  - El submit navega a `/` solo en `isSuccess`.
  - `src/app/router.tsx:7` (comentario TODO con los pasos)
- [ ] **Revisar el guard.** Confirmar que `requireAuth` redirige bien una vez que el store arranca sin sesión, y que el catch-all `*` no genera bucles con `/login`.
  - `src/app/router.tsx:14` (`requireAuth`)
  - `src/app/router.tsx:20` (uso en la rama `/`)
  - `src/app/router.tsx:92` (catch-all `*`)
- [ ] **Agregar `signOut`** en `useAuth`: mutación de logout, `clearSession()`, `queryClient.clear()` y navegar a `/login`.
  - `src/features/auth/hooks/useAuth.ts:6`
- [ ] **Habilitar "Cerrar sesión"** en el menú de usuario (hoy está `disabled`, porque sin `/login` el guard entraría en bucle).
  - `src/app/Header.tsx:80` (comentario TODO)
  - `src/app/Header.tsx:81` (el `DropdownMenuItem disabled`)
- [ ] **Manejar el 401 en el cliente HTTP.** Un 401 debe llamar `useAuthStore.getState().clearSession()` para que el guard mande al usuario a `/login`.
  - `src/lib/apiClient.ts:97` (comentario TODO)
  - `src/lib/apiClient.ts:101` (`throw new ApiError`)

## 2. Backend real del Dashboard (`TODO(backend)`)

- [ ] **Poner el endpoint real.** Reemplazar el texto de relleno.
  - `src/features/dashboard/api/dashboard.ts:6` (comentario TODO)
  - `src/features/dashboard/api/dashboard.ts:7` (`DASHBOARD_ENDPOINT`)
- [ ] **Ajustar los tipos al contrato del backend** (`DashboardSummary`, `KpiValue`, `PendingInvoice`, etc.).
  - `src/features/dashboard/types.ts`
- [ ] **Poner `VITE_USE_MOCKS=false`** cuando el backend exista y, si ya no sirve, borrar `src/features/dashboard/mocks/`.
  - `.env` (local) y `.env.template:8`
  - Rama del mock: `src/features/dashboard/api/dashboard.ts:13`
- [ ] **Poner la URL real del backend** (hoy `http://localhost:8000`) y la API key si aplica.
  - `.env` y `.env.template:2` (`VITE_BASE_API_URL`)

## 3. Moneda y configuración (`TODO(settings)`)

- [ ] **Sacar moneda y locale de Configuraciones > General** en lugar de las constantes fijas `es-HN` / `HNL`.
  - `src/lib/format.ts:1` (comentario TODO)
  - `src/lib/format.ts:3-4` (`LOCALE`, `CURRENCY`)

## 4. Otros TODO sueltos

- [ ] **Enlazar el botón "Ayuda"** a la Central de Ayuda (hoy no hace nada).
  - `src/app/Header.tsx:50` (comentario TODO)
  - `src/app/Header.tsx:53` (el botón "Ayuda")

## 5. Secciones por construir

Hoy son páginas "Próximamente" (`components/ComingSoon.tsx`, usado en la línea 7 de cada archivo). Para cada una: crear `src/features/<nombre>/` (modelo: `features/dashboard`), reemplazar el `ComingSoon` de la página y, si aplica, pasar de mock a endpoint real.

- [ ] **Vender (POS):** buscar productos por nombre/código, carrito, seleccionar cliente, "Ir al pago"; luego descargar recibo, enviar por email o imprimir. → `src/pages/SellPage.tsx:7`
- [ ] **Pedidos:** lista de pedidos abiertos, filtro por artículo/cliente y por estado. Falta ver el flujo de edición. → `src/pages/OrdersPage.tsx:7`
- [ ] **Productos:** alta, edición, categorías, exportar e importar. → `src/pages/ProductsPage.tsx:7`
- [ ] **Catálogo Online:** formulario (nombre del comercio, link del catálogo, identificación, términos). → `src/pages/OnlineCatalogPage.tsx:7`
  - Por definir: ¿basta un POST que crea el sitio o el sistema genera la página?
- [ ] **Clientes:** registro y búsqueda. → `src/pages/CustomersPage.tsx:7`
- [ ] **Transacciones:** historial de ventas, resumen hoy/ayer/semana/mes, búsqueda por cliente/producto. → `src/pages/TransactionsPage.tsx:7`
- [ ] **Finanzas:** cuentas por pagar (atrasados, vence hoy, próximos 7 días) y salidas. → `src/pages/FinancePage.tsx:7`
- [ ] **Estadísticas:** facturación, ventas, ticket medio, ganancia, tasa de venta, medios de pago, top productos/clientes, por período. → `src/pages/StatisticsPage.tsx:7`
- [ ] **Usuarios:** lista con facturación/ventas por usuario, alta de usuario y permisos. → `src/pages/UsersPage.tsx:7`
- [ ] **Configuraciones:** pestañas General, Pedidos y ventas, Recibo, Pagos, Entrega y retirada / Integraciones. → `src/pages/SettingsPage.tsx:7`
  - Entrega y retirada / Integraciones: sin detalle todavía.

## 6. Detalles de producto por decidir

- [ ] **Nombre y logo del sidebar.** Hoy es solo texto.
  - `src/app/Sidebar.tsx:33`
  - `index.html:13` (título de la pestaña)
- [ ] **Commit inicial.** Se hizo `git init` pero no hay commits ni remoto.
