# Error Contract

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | BP §8, §14.3 ("provider errors must not expose secrets") |

## Shape

Canonical schema: [error.schema.json](schemas/error.schema.json).

```json
{
  "error": {
    "code": "MODULE_NOT_ENTITLED",
    "message": "Your current plan does not include this module.",
    "request_id": "0192f4a8-7c1e-7a3b-9d2e-4b5c6d7e8f90",
    "retryable": false,
    "details": [],
    "meta": { "required_entitlement": "module.construction.site-report", "upgrade_available": true }
  }
}
```

## Rules

1. `code` is stable and machine-readable; clients branch on `code`, never on `message`.
2. `code` namespaces: `AUTH_`, `ORG_`, `ACCESS_`, `MODULE_`, `SOURCE_`, `UPLOAD_`, `JOB_`, `KNOWLEDGE_`, `AI_`, `ARTIFACT_`, `TEMPLATE_`, `WORKFLOW_`, `ACTION_`, `BILLING_`, `INTEGRATION_`, `SECURITY_`, `PRIVACY_`, `VALIDATION_`, `IDEMPOTENCY_`, `RATE_`, `SYSTEM_`.
3. `message` is localised by `Accept-Language` and safe to display. It never contains stack traces, SQL, provider raw errors, system prompts, secrets or other tenants' data.
4. `details[]` lists field-level problems with JSON Pointer `field`.
5. `retryable` indicates whether retrying later can succeed.
6. Every error is logged with `request_id` and the internal cause; only the public projection is returned.
7. HTTP status and code are paired as defined in [error-catalog](error-catalog.md); a code always maps to the same status.
8. Domain errors in Go are typed (`platform/errors`) and mapped in one place per transport; unmapped errors become `500 SYSTEM_INTERNAL_ERROR`.

## Status Code Usage

| Status | Use |
|---|---|
| 400 | Malformed syntax, missing required header |
| 401 | Unauthenticated |
| 402 | Entitlement/quota (commercial) failure |
| 403 | Permission/policy denial on a visible resource |
| 404 | Not found or not visible (tenant/scope) |
| 409 | State conflict, invalid transition, duplicate, idempotency in progress |
| 410 | Expired (upload session, invitation) |
| 412 / 428 | If-Match mismatch / missing |
| 413 / 415 | Size / media type |
| 422 | Semantically invalid (schema violations, readiness failures, idempotency key reuse) |
| 429 | Rate limited |
| 500 | Unexpected |
| 503 | Dependency unavailable / read-only mode (`retryable: true`) |
