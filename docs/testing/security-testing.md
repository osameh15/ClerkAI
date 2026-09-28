# Security Testing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security + QA |
| Blueprint | BP §19 (Security: Authorization matrix, BOLA, upload abuse, rate limits, injection), §14.4 |

| Suite | Content | When |
|---|---|---|
| Authorization matrix | Generated from OpenAPI annotations × role matrix | Every PR |
| BOLA / cross-tenant | Foreign IDs on all `{id}` routes; RLS tests | Every PR |
| Upload abuse | Wrong MIME, polyglots, zip bombs, huge images, EICAR, oversized, expired URLs | Every PR (subset), nightly (full) |
| Rate limits | Exceed limits per scope | Nightly |
| Injection | SQL (parameterisation assertions), header injection, XSS payloads in content/filenames, prompt injection corpus | Nightly |
| SSRF | Webhook URLs to private/metadata/rebinding domains | Every PR (integrations) |
| Auth | Magic link reuse/expiry, session fixation, CSRF, MFA bypass attempts | Nightly |
| Secrets | Secret scanning (repo + images) | Every PR |
| SAST / deps / containers / IaC | Tooling | Every PR |
| DAST | Staging scans | Weekly from Private Beta |
| Penetration test | External | Before Paid Launch (BP §14.4) |
