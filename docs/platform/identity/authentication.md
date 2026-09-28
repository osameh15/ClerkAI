# Authentication (Identity Domain)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (provider implementation Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Identity domain |
| Blueprint | BP §14.2, App. A (Authentication), App. F (Sprint 0 Identity) |

## Scope

Sign-in methods, MFA, and the `Authenticator` port. HTTP contract: [api/authentication](../../api/authentication.md). Sessions: [sessions](sessions.md). Providers: [identity-providers](identity-providers.md).

## Methods

| Method | v1 | Notes |
|---|---|---|
| Magic link (email) | Yes | Single-use token, 15 min TTL, hashed at rest, bound to requesting browser via cookie nonce |
| OIDC (Google, Microsoft, generic) | Yes | Authorization Code + PKCE, `state`, `nonce`; email must be verified by IdP |
| Password | No (not planned) | If ever added: Argon2id, breach check (BP §14.2) |
| SAML SSO | Architecture-ready | Enterprise (`sso.saml`) |
| Local/dev provider | Sprint 0 only | Behind `Authenticator` interface, disabled in staging/production by config guard |

## MFA

- Factors: TOTP (v1), WebAuthn/passkeys (v1 if [OD-004](../../governance/open-decisions.md#od-004) provider supports; otherwise v1.x).
- Required for Owner/Admin role holders and when organization policy enforces MFA (Enterprise). Enforcement point: privileged operations require `session.mfa_level = mfa`.
- Recovery codes (10, single-use, hashed).

## Domain Model

`User` (global, not tenant-owned) · `AuthIdentity` (provider + subject) · `Session` · `MFAFactor` · `MagicLinkToken`. Users link multiple identities by verified email; linking a new IdP to an existing email requires sign-in with an existing method (prevents account takeover through unverified IdPs).

## Port

```go
type Authenticator interface {
    StartMagicLink(ctx context.Context, email string, redirect string) error
    VerifyMagicLink(ctx context.Context, token string) (UserID, error)
    StartOIDC(ctx context.Context, provider string) (redirectURL string, err error)
    CompleteOIDC(ctx context.Context, provider, code, state string) (UserID, error)
}
```

## Events and Audit

Events: `identity.user.created`, `identity.session.created`, `identity.session.revoked`, `identity.mfa.enrolled`. Audit: sign-in success/failure, MFA enrol/remove, session revoke, identity link/unlink ([audit-event-catalog](../audit/audit-event-catalog.md)).

## Security

Uniform responses for existing/non-existing emails; rate limits per IP/email; tokens compared in constant time; no user enumeration ([authentication-security](../../security/authentication-security.md)).
