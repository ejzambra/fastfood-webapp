# Esquema inicial DynamoDB

## Objetivos de acceso

1. Garantizar **unicidad de cuenta abierta por cliente**.
2. Consultar rápidamente **tickets de cocina** por estado/prioridad/tiempo.
3. Mantener **histórico inmutable de cierres y correcciones de pago**.

---

## Tabla 1: `RestaurantOps`

Tabla single-table para operaciones del negocio (clientes, cuentas, pedidos, items, pagos, inventario, config).

### Claves

- **PK** (string)
- **SK** (string)

### Entidades principales

- Cliente:
  - `PK = TENANT#{tenantId}#CUSTOMER#{customerId}`
  - `SK = PROFILE`
- Cuenta:
  - `PK = TENANT#{tenantId}#ACCOUNT#{accountId}`
  - `SK = HEADER`
- Pedido:
  - `PK = TENANT#{tenantId}#ACCOUNT#{accountId}`
  - `SK = ORDER#{orderId}`
- Item:
  - `PK = TENANT#{tenantId}#ACCOUNT#{accountId}`
  - `SK = ITEM#{orderId}#{itemId}`
- Pago (evento):
  - `PK = TENANT#{tenantId}#ACCOUNT#{accountId}`
  - `SK = PAYMENT_EVT#{timestamp}#{paymentEventId}`
- Cierre (evento):
  - `PK = TENANT#{tenantId}#ACCOUNT#{accountId}`
  - `SK = CLOSE_EVT#{timestamp}#{closeEventId}`
- Corrección pago (evento):
  - `PK = TENANT#{tenantId}#ACCOUNT#{accountId}`
  - `SK = PAYMENT_CORR_EVT#{timestamp}#{correctionId}`
- Movimiento inventario:
  - `PK = TENANT#{tenantId}#BRANCH#{branchId}#INVENTORY`
  - `SK = MOV#{timestamp}#{movementId}`
- Config cocina:
  - `PK = TENANT#{tenantId}#BRANCH#{branchId}#KITCHEN_CFG`
  - `SK = ACTIVE`

### GSIs

#### `GSI1` — Unicidad cuenta abierta por cliente

- **GSI1PK**: `TENANT#{tenantId}#CUSTOMER#{customerId}#OPEN_ACCOUNT`
- **GSI1SK**: `BRANCH#{branchId}`
- Solo se proyecta en cuenta con estado `OPEN`.
- Validación de unicidad: escritura condicional `attribute_not_exists(GSI1PK)` en apertura.

#### `GSI2` — Tickets de cocina

- **GSI2PK**: `TENANT#{tenantId}#BRANCH#{branchId}#KITCHEN`
- **GSI2SK**: `STATUS#{status}#PRIO#{priority}#TS#{createdAt}#ITEM#{itemId}`
- Proyectar atributos: `accountId`, `orderId`, `itemName`, `qty`, `notes`, `status`.

#### `GSI3` — Búsqueda de cuentas por estado/fecha

- **GSI3PK**: `TENANT#{tenantId}#BRANCH#{branchId}#ACCOUNT_STATUS#{status}`
- **GSI3SK**: `TS#{openedAt}` (o `closedAt` según estado final)

---

## Tabla 2: `AccountLedger`

Tabla append-only para auditoría financiera y cierres (opcionalmente separada por cumplimiento).

### Claves

- `PK = TENANT#{tenantId}#ACCOUNT#{accountId}`
- `SK = EVT#{timestamp}#{eventType}#{eventId}`

### Tipos de evento

- `PAYMENT_APPLIED`
- `PAYMENT_CORRECTED`
- `ACCOUNT_CLOSED`
- `ACCOUNT_MARKED_UNCOLLECTIBLE`

### Índices

- `GSI1PK = TENANT#{tenantId}#BRANCH#{branchId}#LEDGER_EVT#{eventType}`
- `GSI1SK = TS#{timestamp}`

Con esto se soporta reporte histórico por sucursal/tipo de evento sin escanear la tabla completa.

---

## Patrones de acceso cubiertos

1. **Abrir cuenta de cliente**
   - Escribir item `ACCOUNT#HEADER` con condición de unicidad en `GSI1`.
2. **Obtener tickets de cocina activos**
   - Query `GSI2PK = ...#KITCHEN` + begins_with `STATUS#PENDING` o `STATUS#IN_PREP`.
3. **Cerrar cuenta y registrar pago/corrección**
   - Append events en `RestaurantOps` y/o `AccountLedger` (sin sobrescribir).
4. **Auditar correcciones/incobrables**
   - Query por PK de cuenta o por GSI de eventos financieros.
