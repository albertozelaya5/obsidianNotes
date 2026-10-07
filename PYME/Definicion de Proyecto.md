El proyecto es para ver la facturación de las PYMES, va a usar Typescript, zustand, react query, tailwind

La pantalla principal sera un Dashboard que muestre facturas pendientes, ingresos, resumen de facturas y resumen de ventas

- Vender — pantalla de punto de venta (POS). Buscás productos por nombre/código, los agregás al carrito, seleccionás cliente y procesás el cobro ("Ir al pago"), una vez pagado, se puede descargar el recibo, enviar por email o imprimir
- Pedidos — lista de pedidos abiertos/pendientes (distinto de una venta directa en "Vender"). Se puede filtrar por artículo/cliente y por estado, esta no la pude ver mucho porque no me permite editarla xd
- Productos — catálogo de artículos del negocio: alta, edición, categorías, exportar e importar productos. (registro de productos)
- Catálogo Online — genera una tienda web pública (link tipo tunombre.kyte.site) para que los clientes vean/compren productos online. Pide nombre del comercio e identificación fiscal. - esto no se si es que solo te hago un POST para crear el sitio y ya, o shit si te crea la pagina
- Clientes — CRM básico: registro y búsqueda de clientes para asociarlos a ventas/pedidos.
- Transacciones — historial de ventas con resumen rápido (hoy, ayer, semana, mes) y búsqueda por cliente/producto.
- Finanzas — control de cuentas por pagar (gastos con vencimiento) y registro de salidas de dinero.
- Estadísticas — dashboard de métricas: facturación, cantidad de ventas, ticket medio, ganancia, tasa de venta, medios de pago y productos/clientes top, filtrable por período.
- Usuarios — gestión de usuarios del sistema (owner, empleados) con visibilidad de facturación/ventas por usuario y permisos.
- Configuraciones — ajustes generales del negocio, divididos en pestañas:
    - General: datos de identificación, contacto, dirección, logo, moneda.
    - Pedidos y ventas: tasas/impuestos de venta y estados personalizables del flujo de pedidos.
    - Recibo: personalización del ticket/recibo impreso (datos de tienda, encabezado/pie).
    - Pagos: métodos de pago aceptados (saldo cliente/fiado, link de pago, pagos automáticos, lectores de tarjeta).
    - Entrega y retirada / Integraciones: opciones de logística y conexiones externas (no capturadas en detalle aquí).

En resumen: es un sistema tipo POS + e-commerce simple para PyMEs — vender, gestionar pedidos/productos/clientes, ver finanzas y estadísticas, y configurar el negocio.

Todas las secciones estan sujetas a cambios, por lo que debe ser un proyecto escalable