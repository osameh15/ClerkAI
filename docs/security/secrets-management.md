# Secrets Management

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (manager Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security + DevOps |
| Blueprint | BP §14.2 ("Secrets stored in a Secret Manager; no API key in plaintext DB fields or source code") |

## Rules

- No secrets in Git (pre-commit + CI secret scanning; blocking).
- Secret manager ([OD-013](../governance/open-decisions.md#od-013)) per environment; workloads authenticate with workload identity; least-privilege paths per service/pool.
- AI provider credentials: one secret per provider per environment; only the `ai` worker pool can read them.
- Webhook signing secrets and customer secrets: stored in secret manager or envelope-encrypted in DB (`secret_ref`); never logged.
- Local development: `.env.local` (gitignored) with sandbox/mock credentials only.
- Rotation: documented per secret class (DB passwords 90 days or dynamic credentials; provider keys on staff change or suspicion; webhook secrets customer-driven).
- Access to production secrets is audited and restricted to on-call SRE via break-glass.

See also [operations/secrets](../operations/secrets.md).
