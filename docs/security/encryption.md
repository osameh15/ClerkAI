# Encryption

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (KMS provider Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security + DevOps |
| Blueprint | BP §14.2 ("TLS for all network traffic; encryption at rest for DB/Object Storage and KMS-managed keys in Production") |

| Area | Control |
|---|---|
| In transit | TLS 1.2+ (1.3 preferred) externally; TLS internally between services and to data stores where supported; HSTS |
| Database at rest | Storage-level encryption (managed service or encrypted volumes) with KMS keys in production |
| Object storage at rest | SSE with KMS keys; separate keys per bucket class (sources, exports, ai-payloads, quarantine) |
| Backups | Encrypted with separate backup keys |
| Application-level encryption | Envelope encryption (data key per record/tenant, wrapped by KMS) for: MFA secrets, webhook secrets (if not in secret manager), OAuth refresh tokens (future integrations), stored AI payloads |
| Hashing | API keys: HMAC-SHA-256 with pepper; session tokens/magic links: SHA-256; passwords (if any): Argon2id |
| Key rotation | KMS automatic rotation yearly; data keys re-wrapped; webhook/API secrets rotatable by customers |
| Offline device | Browser storage protection per [OD-023](../governance/open-decisions.md#od-023) |
| Enterprise (future) | Customer-managed keys (BYOK) — architecture-ready via per-tenant data keys |
