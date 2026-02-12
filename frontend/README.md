# Frontend (SPA)

SPA responsive para tres vistas principales:

- **Mozo**: apertura de cuenta por cliente, carga y seguimiento de pedidos, cierre/cobro.
- **Cocina**: tickets en tiempo real, cambios de estado de items y control de refresco.
- **Admin**: inventario, desperdicio, auditoría de pagos/cierres y métricas operativas.

## Estructura mínima sugerida

- `src/app/`: bootstrap y routing por roles.
- `src/features/waiter/`: flujo de mozo.
- `src/features/kitchen/`: tablero de cocina.
- `src/features/admin/`: panel administrativo.
- `src/shared/`: componentes reutilizables y utilidades.
