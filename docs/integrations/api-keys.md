# API Keys (Integration Domain)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Integrations domain |
| Blueprint | BP §8, §14.1, §14.2 |

Format, scopes and lifecycle are canonical in [api/api-keys](../api/api-keys.md). Operational notes:

- Keys belong to the organization; created by a member with `integrations.manage`; recorded creator for permission intersection.
- Admin UI shows prefix, name, scopes, last used, expiry; never the secret after creation.
- Usage metered under `api` meter per key; rate limits per key.
- Automatic suspension when the creator's membership is removed (Owner can re-assign ownership).
- Audit: creation, revocation, scope changes, suspensions.
