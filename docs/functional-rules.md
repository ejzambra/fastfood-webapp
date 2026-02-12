# Reglas funcionales iniciales

## 1. Clientes y cuentas abiertas

- Una cuenta abierta activa por cliente por sucursal.
- No se permite abrir nueva cuenta si existe una en estado `OPEN`.
- Una cuenta cerrada no admite nuevos items.

## 2. Pedidos e items

- Estados de item: `PENDING -> IN_PREP -> READY -> DELIVERED`.
- Un item puede anularse con estado `CANCELLED` antes de `DELIVERED`.
- Los pedidos generan tickets visibles en cocina según prioridad y timestamp.

## 3. Pagos y cierre

- La cuenta pasa a `CLOSED` sólo si el balance final es cero o se marca `UNCOLLECTIBLE`.
- Correcciones de pago no reescriben eventos previos: se registran como ajuste/auditoría.
- Debe existir trazabilidad de usuario/fecha/motivo para correcciones e incobrables.

## 4. Inventario y desperdicio

- Cada venta puede descontar inventario según receta/insumo configurado.
- Desperdicio siempre se registra con causa y responsable.
- No se permiten cantidades negativas en movimientos.

## 5. Refresco de cocina

- Configuración por sucursal con valor en segundos (`refreshSeconds`).
- Rango permitido inicial: 5 a 120 segundos.
- Cambios de configuración quedan auditados (quién, cuándo, valor previo/nuevo).
