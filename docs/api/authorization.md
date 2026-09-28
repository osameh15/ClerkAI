# API Authorization

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) + Security |
| Blueprint | BP §13, §14.2, App. C |

## Purpose

How HTTP endpoints apply the platform authorization model. The canonical model is [authorization-model](../platform/access/authorization-model.md).

## Evaluation Order per Request

```text
Authenticated → Tenant access → Entitlement → Permission → Resource policy → Allowed
```

| Step | Failure | HTTP | Code |
|---|---|---|---|
| Authenticated | No/invalid credential | 401 | `AUTH_REQUIRED` |
| Tenant access | No active membership / key not bound to org | 403 (or 404 for resource paths) | `ORG_ACCESS_DENIED` |
| Entitlement | Organization lacks entitlement or quota | 402 | `MODULE_NOT_ENTITLED`, `BILLING_ENTITLEMENT_REQUIRED`, `BILLING_QUOTA_EXCEEDED` |
| Permission | Role lacks permission | 403 | `ACCESS_PERMISSION_DENIED` |
| Resource policy | Outside team/project/ownership scope | 404 if not visible, else 403 | `ACCESS_RESOURCE_DENIED` |

`402` is used deliberately for entitlement failures so the client can render an **upgrade** state rather than an access-denied state ([permissions-ui](../frontend/permissions-ui.md), [billing-ui](../frontend/billing-ui.md)).

## Endpoint Annotations

OpenAPI operations declare `x-clerk-permission` and `x-clerk-entitlement`. These annotations are documentation and test inputs; enforcement happens in use cases (never only in middleware) so object-level checks see the loaded resource. Module-specific permissions (for example `construction.site-report.create`) are resolved dynamically from the Job's module: creating a Job requires both `jobs.create` and `<module-key>.create`.

## API Key Scopes

API keys carry scopes (`jobs:write`, `artifacts:read` …) that further **restrict** the organization's permissions; a key can never exceed the permissions of the role it was created under ([api-keys](api-keys.md)).

## Tests

Authorization matrix tests are generated from the OpenAPI annotations × [permission-matrix](../platform/access/permission-matrix.md) and run in CI; BOLA tests use cross-tenant IDs on every `{id}` path ([security-testing](../testing/security-testing.md)).
