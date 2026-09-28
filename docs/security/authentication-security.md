# Authentication Security

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security + Identity |
| Blueprint | BP §14.2, §14.4 (Enterprise: SSO/MFA) |

| Control | Requirement |
|---|---|
| Credentials | No passwords in v1; magic links single-use, 15-min TTL, hashed; OIDC with PKCE/state/nonce |
| Passwords (if ever) | Argon2id (memory ≥ 64 MiB, iterations ≥ 3, parallelism tuned), breached-password check, no composition rules beyond length ≥ 12 |
| MFA | Required for Owner/Admin; org-enforceable for all; TOTP + recovery codes; WebAuthn preferred when available |
| Sessions | Opaque tokens (256-bit), hashed at rest, `__Host-` cookie, rotation on privilege change, idle/absolute timeouts, device list, revoke |
| Re-authentication | Sensitive operations (ownership transfer, org deletion, MFA removal, API key creation) require recent auth (≤ 15 min) |
| Brute force | Rate limits per IP/email; exponential backoff; generic responses (no enumeration) |
| Account linking | Only verified emails; linking requires existing session |
| Anomaly detection | New country/device sign-in notification; impossible travel flag (provisional) |
| Audit | All auth events audited ([audit-event-catalog](../platform/audit/audit-event-catalog.md)) |
