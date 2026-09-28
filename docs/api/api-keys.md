# API Keys

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Integrations + Security |
| Blueprint | BP §8, §14.1, §14.2 ("no API key in plaintext DB fields"), App. B, App. C (API/Webhooks entitlement) |

## Format

`ck_{env}_{prefix}_{secret}` — `env ∈ {live, test}`, `prefix` 8 random base32 chars (stored plain, shown in UI, used for lookup), `secret` 32 random bytes base32. Only `HMAC-SHA-256(pepper, secret)` (pepper in secret manager) is stored; lookup by `prefix` then constant-time hash compare.

## Scopes

`<resource>:<read|write>` — `jobs`, `sources`, `artifacts`, `actions`, `webhooks`, `usage` (read only), `audit` (read, requires audit entitlement). A key's effective rights = scopes ∩ permissions of the creator's role at creation, re-evaluated if the creator loses permissions (key is suspended if the creator is removed).

## Lifecycle

Create (secret shown once) → use (`last_used_at` updated asynchronously) → optional expiry → revoke. All operations audited. Keys are bound to one organization and never usable across orgs.

## Rate Limits

Per-key limits in [rate-limiting](rate-limiting.md).

## Leak Response

If a key appears leaked (secret scanning partner, customer report), it is revoked immediately and the Owner notified ([incident-response](../operations/incident-response.md)).
