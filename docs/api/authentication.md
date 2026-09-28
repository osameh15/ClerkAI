# API Authentication

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (IdP implementation Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) + Security |
| Blueprint | BP §14.2, App. A (Authentication), App. B |

## Principals

| Principal | Credential | Used by | Tenant binding |
|---|---|---|---|
| User (session) | `__Host-clerk_session` cookie (HttpOnly, Secure, SameSite=Lax, Path=/) | Nuxt PWA | Active org via `Clerk-Organization-Id` header |
| API key | `Authorization: Bearer ck_live_…` | Integrations | Bound to exactly one organization |
| System | Internal workload identity | Workers | From workflow input |
| Remote module (future) | Signed requests via Remote Module Gateway | Marketplace M2 | Job-scoped grant |

## Session Flow (web)

1. Sign-in by magic link or OIDC ([OD-004](../governance/open-decisions.md#od-004)); password auth, if ever added, uses Argon2id (BP §14.2).
2. Server creates `sessions` row; cookie holds an opaque random token (≥ 256 bits); DB stores only its hash.
3. MFA required for Owner/Admin roles and Enterprise policies (BP §14.2); a session is `mfa_level=none|mfa` and privileged endpoints require `mfa`.
4. Session rotation on sign-in, privilege change and MFA completion; idle timeout (default 7 days) and absolute timeout (default 30 days) — provisional.
5. CSRF: unsafe methods require `X-CSRF-Token` equal to a per-session token delivered via `GET /v1/me`; plus `SameSite=Lax` and strict Origin check.

Canonical identity design: [platform/identity/authentication](../platform/identity/authentication.md), [sessions](../platform/identity/sessions.md).

## API Keys

See [api-keys](api-keys.md). API keys never authenticate browser sessions and never access `/v1/me/*` or identity endpoints.

## Errors

`401 AUTH_REQUIRED` (no/invalid credential), `401 AUTH_SESSION_EXPIRED`, `401 AUTH_MFA_REQUIRED`, `403 AUTH_API_KEY_SCOPE_INSUFFICIENT` — see [error-catalog](error-catalog.md).
