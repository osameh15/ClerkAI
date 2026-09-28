# Webhook Security (API Contract)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security + Integrations |
| Blueprint | BP §8.3, §14.1 (Webhook/API abuse) |

## Receiver Verification (documented for customers)

1. Read `Clerk-Timestamp` and `Clerk-Signature`.
2. Reject if `|now − timestamp| > 300 s` (replay window).
3. Compute `HMAC-SHA256(secret, timestamp + "." + raw_body)`; compare in constant time with each `v1=` value (multiple during rotation).
4. Deduplicate by `Clerk-Event-Id`.

## Platform Controls

| Threat | Control |
|---|---|
| SSRF via endpoint URL | HTTPS only; DNS resolution checked at registration **and** each delivery; block private, loopback, link-local, metadata ranges; egress proxy with allowlist semantics ([security/webhook-security](../security/webhook-security.md)) |
| Secret leakage | Secret generated server-side (≥ 32 bytes), shown once, stored in secret manager/envelope-encrypted; rotation with 24 h overlap |
| Replay | Timestamped signature + event id |
| Payload data leakage | Minimal public payload (IDs, statuses); no content |
| Abuse (endpoint DoS) | Per-endpoint concurrency 4; backoff; disable on persistent failure |
| Redirect tricks | Redirects not followed |
