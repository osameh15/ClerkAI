# Authorization Security

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security + Access |
| Blueprint | BP §13, §14.2 ("Object-level authorization on every endpoint; possession of an ID is never authorization") |

## Rules

1. Deny by default: an endpoint without declared permission annotations fails CI.
2. Object-level checks in use cases after loading the resource (never trust IDs from clients).
3. Function-level checks: admin endpoints require admin permissions and MFA session.
4. Property-level: response DTOs filter fields per obligations (for example transcript) — no mass assignment (request DTOs with explicit allowed fields, `additionalProperties: false`).
5. Approval separation of duties enforced by workflow policy.
6. No authorization decisions in the frontend beyond hiding UI.
7. API keys: scope ∩ creator permissions.

## Tests

Generated authorization matrix tests; BOLA tests with cross-tenant and cross-scope IDs; negative tests for every role; privilege escalation tests for custom roles.
