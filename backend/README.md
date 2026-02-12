# Backend (Lambdas + API Gateway)

Base para APIs serverless en AWS Lambda.

## Contenido inicial

- `contracts/openapi.yaml`: contratos API iniciales para dominio de restaurante.

## Próximos pasos sugeridos

1. Implementar handlers por bounded context:
   - cuentas/clientes
   - pedidos/cocina
   - pagos/cierre
   - inventario
   - configuración
2. Agregar validación de schemas de entrada/salida.
3. Integrar autenticación/autorización con Cognito (claims por rol).
