---
technologies: [aws]
---

# AWS Coding Rules

## boto3
- Always pass `region_name` explicitly — never rely on environment defaults.
- Use resource APIs (`boto3.resource`) over client APIs (`boto3.client`) where available.
- Always set `connect_timeout` and `read_timeout` via `botocore.config.Config`.

## Lambda
- Handler signature: `def handler(event: dict, context: Any) -> dict`.
- Keep handlers thin — business logic belongs in modules, not the handler.
- Log the incoming event at DEBUG level (not INFO — avoids leaking PII in prod).
- Return structured responses with `statusCode` and `body` for API Gateway integrations.

## DynamoDB
- Use single-table design with a `PK`/`SK` composite key pattern.
- Prefer `boto3.resource('dynamodb').Table(name)` over the low-level client.
- Always handle `ConditionalCheckFailedException` for conditional writes.
- Use `Decimal` (not `float`) for numeric attributes — boto3 requires it.

## IAM
- Apply least-privilege. Lambda execution roles should only have the permissions the function needs.
- Never hardcode credentials. Use IAM roles for Lambda, `~/.aws/credentials` for local dev.

## Secrets
- Store secrets in SSM Parameter Store (SecureString) or Secrets Manager, never in env vars or code.
