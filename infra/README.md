# Infraestructura (IaC)

Directorio para definir infraestructura AWS:

- Hosting frontend: **S3 + CloudFront**
- Auth: **Cognito**
- APIs: **API Gateway + Lambda**
- Persistencia: **DynamoDB**
- Observabilidad: **CloudWatch**
- Costos: **AWS Budgets**

## Estructura sugerida

- `modules/frontend-hosting`
- `modules/auth-cognito`
- `modules/api-lambda`
- `modules/data-dynamodb`
- `modules/observability-cloudwatch`
- `modules/cost-budgets`
- `envs/dev`, `envs/staging`, `envs/prod`
