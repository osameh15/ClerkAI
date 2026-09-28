# Data Classification

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Privacy + Security |
| Blueprint | BP §15.1 |

## Classes (canonical, BP §15.1)

| Class | Example | Policy |
|---|---|---|
| Public | Public Module Store listing | Standard controls |
| Internal | Non-sensitive operational telemetry | Restricted internal access |
| Confidential | Meeting minutes, project reports (default for all customer content) | Tenant-scoped + encryption + audit |
| Restricted | PII, sensitive contracts, Enterprise files, credentials/secrets, MFA secrets | Strong access, retention, export logging, possible regional restrictions |

## Default Classification by Data Type

| Data | Class |
|---|---|
| Sources (raw), extractions, chunks, embeddings | Confidential |
| Knowledge, artifacts, exports | Confidential |
| User profile (email, name), sessions, IPs | Restricted (PII) |
| Participant names inside content | Confidential (PII within content) |
| AI raw payloads | Confidential (Restricted if org marks) |
| Audit events | Internal/Restricted |
| Usage events, billing | Internal (billing PII Restricted) |
| Secrets, API key hashes, webhook secrets | Restricted |
| Evaluation datasets (synthetic) | Internal; (consented production-derived) Restricted |

## Organization-Level Marking

Organizations/projects may mark data as Restricted (Enterprise, `ai.private_policy`): effects — AI routes limited to allowed providers/regions, payload storage disabled, link sharing disabled, export logging mandatory.
