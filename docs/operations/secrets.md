# Secrets (Operations)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (manager Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | DevOps |
| Blueprint | BP §14.2, §18 |

Security policy: [secrets-management](../security/secrets-management.md).

## Operational Procedures

| Procedure | Steps |
|---|---|
| Add secret | Create in secret manager under `clerk/{env}/{service}/{name}`; grant to workload identity; reference in deploy config |
| Rotate | Create new version → deploy/reload → verify → revoke old version; dual-validity window for signing keys |
| Emergency revoke | Revoke at provider → rotate → incident record |
| Access review | Quarterly review of who/what can read production secrets |
| CI secrets | Scoped per pipeline/environment; never printed; masked |

## Inventory (initial)

Database credentials (per role), Redis auth, object storage credentials, Temporal mTLS certs, KMS key references, AI provider keys (per provider), payment provider keys + webhook signing secret, email provider key, session/CSRF/cursor HMAC keys, API-key pepper, OTel exporter credentials, malware scanner update credentials.
