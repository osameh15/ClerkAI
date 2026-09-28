# Enterprise Data Controls

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline (architecture-ready; implementation on demand) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + Privacy |
| Blueprint | BP §2 (Enterprise IT/Security), §5.1 (Enterprise), §27 ("Early Enterprise demands: architecture-ready"), App. A (AI Policy), App. C |

| Control | Entitlement | v1 status |
|---|---|---|
| Custom retention per category | `retention.custom` | Implemented (Business limited, Enterprise full) |
| Disable AI payload storage | `ai.private_policy` | Implemented |
| Provider/region allowlist | `ai.private_policy` | Implemented |
| Training/evaluation use prohibition | contract | Implemented (policy lock) |
| Redaction before provider calls | `ai.private_policy` | Architecture-ready |
| SSO/SAML enforcement | `sso.saml` | Architecture-ready |
| Audit export | `audit.export` | Implemented |
| Data residency | contract | Architecture-ready ([OD-028](../governance/open-decisions.md#od-028)) |
| Customer-managed keys | contract | Future |
| Legal hold | contract | Implemented (policy flag) |
| Human review defaults enforcement | `ai.private_policy` | Implemented |

Configured on the AI Policy and Security screens by Owners/Admins with `privacy.manage` / `security.manage`.
