# Audit Security

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security |
| Blueprint | BP §14.1 (Privileged misuse), §14.2 (Immutable audit events), §14.4 (Enterprise: audit export) |

| Control | Implementation |
|---|---|
| Append-only | `clerk_app` INSERT only; triggers block UPDATE/DELETE; only retention procedures (separate role) delete by policy |
| Tamper evidence | Per-organization hash chain: `hash = SHA-256(prev_hash ‖ canonical(event))`; daily anchor hash written to a separate store (and optionally exported) |
| Completeness | Audit written in the same transaction as the change (cannot commit without audit) |
| Access | Audit queries are themselves audited for exports; platform-admin access audited separately |
| Export | JSONL/CSV with integrity manifest (hash chain anchors) for Enterprise |
| Retention floors | Security events ≥ 365 days for Business; longer for Enterprise (BP §15.2) |
| Content | No customer content, secrets or tokens in audit rows |
| Verification | Scheduled job verifies hash chains; alert on break |
