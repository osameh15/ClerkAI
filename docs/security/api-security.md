# API Security

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security + Tech Lead |
| Blueprint | BP §14.2, R4 (OWASP API Security Top 10 2023) |

## OWASP API Top 10 (2023) Mapping

| Risk | Control |
|---|---|
| API1 BOLA | Object-level authz in use cases; 404 for invisible; BOLA tests |
| API2 Broken Authentication | Session/API key design; rate limits on auth; MFA |
| API3 Broken Object Property Level Authorization | Explicit DTOs; `additionalProperties: false`; field obligations |
| API4 Unrestricted Resource Consumption | Rate limits, size limits, pagination caps, quotas, AI budgets |
| API5 Broken Function Level Authorization | Permission annotations mandatory; admin path separation |
| API6 Unrestricted Access to Sensitive Business Flows | Idempotency, per-command limits (submit/export), anomaly detection |
| API7 SSRF | No user-supplied URLs fetched except webhooks via guarded egress |
| API8 Security Misconfiguration | Hardened defaults, security headers, IaC policy checks |
| API9 Improper Inventory Management | OpenAPI as the single inventory; undocumented routes fail CI |
| API10 Unsafe Consumption of APIs | Provider/payment responses validated; timeouts; typed errors |

## Browser-Facing Headers

`Content-Security-Policy` (strict, nonces for scripts, `frame-ancestors 'none'`), `Strict-Transport-Security`, `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy` (camera/microphone only for app origin), `Cross-Origin-Opener-Policy: same-origin`.

## CORS

Allowlist of first-party origins; credentials allowed only for first-party; API-key clients use no-cookie requests.

## CSRF

Cookie-authenticated unsafe methods require `X-CSRF-Token` + Origin check; SameSite=Lax.
