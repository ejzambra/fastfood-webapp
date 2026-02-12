# FastFood WebApp

Estructura inicial para un sistema de gestión de fast-food con SPA, APIs serverless e infraestructura en AWS.

## Mapa de módulos

- `frontend/`: SPA responsive para roles de **mozo**, **cocina** y **admin**.
- `backend/`: contratos API y base para Lambdas.
- `infra/`: definición de infraestructura como código (IaC) para recursos AWS.
- `docs/`: decisiones de arquitectura y reglas funcionales.

## Ejecución local (mínima)

> Esta base es un esqueleto de proyecto. Los comandos son punto de partida para completar en siguientes iteraciones.

### 1) Frontend

```bash
cd frontend
# sugerido: crear app Vite/React en la siguiente iteración
npm install
npm run dev
```

### 2) Backend

```bash
cd backend
# sugerido: usar SAM o Serverless Framework
npm install
npm run dev
```

### 3) Infraestructura

```bash
cd infra
# ejemplo con Terraform (cuando se agreguen .tf)
terraform init
terraform plan
```

## Documentación clave

- Contratos API iniciales: `backend/contracts/openapi.yaml`
- Esquema DynamoDB y patrones de acceso: `docs/dynamodb-schema.md`
- Reglas funcionales y decisiones: `docs/functional-rules.md`
