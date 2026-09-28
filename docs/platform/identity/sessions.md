# Sessions

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Identity domain |
| Blueprint | BP §14.2 ("Session rotation, device/session revoke"), App. A (Session Management) |

## Design

- Server-side sessions: `sessions` table in PostgreSQL is the record (revocation, audit); Redis caches validation ([OD-034](../../governance/open-decisions.md#od-034)).
- Cookie `__Host-clerk_session`: opaque 256-bit random token; DB stores SHA-256 hash; `HttpOnly; Secure; SameSite=Lax; Path=/`.
- Timeouts (provisional): idle 7 days, absolute 30 days; privileged operations require re-authentication within 15 minutes (`SECURITY_REAUTH_REQUIRED`).
- Rotation: new token on sign-in, MFA completion, privilege change; old token invalidated.
- Revocation: user can revoke own sessions (`DELETE /v1/me/sessions/{id}`); admins with `security.manage` can revoke sessions of members **for their organization context** (session marked restricted for that org); platform can revoke globally.
- Active organization is not stored in the session; it is sent per request (`Clerk-Organization-Id`), enabling multiple tabs with different workspaces.
- Device info stored minimally (UA hash, IP prefix) for the session list.

## Events and Audit

`identity.session.created`, `identity.session.revoked`; audited: creation, revocation, re-authentication for privileged action.
