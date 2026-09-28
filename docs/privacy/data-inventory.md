# Data Inventory

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Privacy |
| Blueprint | BP §15 |

| Data category | Personal data? | Purpose | Store | Retention category | Shared with |
|---|---|---|---|---|---|
| Account (email, name, locale) | Yes | Authentication, collaboration | PostgreSQL | account lifetime | Email provider (transactional) |
| Sessions, IP prefix, UA hash | Yes | Security | PostgreSQL/Redis | operational (≤ 90 d after expiry) | — |
| Organization profile | Low | Tenancy, billing | PostgreSQL | org lifetime | Payment provider |
| Billing identity, invoices | Yes | Billing, legal | PostgreSQL + payment provider | legal (per jurisdiction) | Payment provider |
| Raw audio/video | Likely (voices) | Processing | Object storage | `raw_media` | AI providers (transcription) |
| Raw images/documents | Likely | Processing, evidence | Object storage | `raw_documents` | AI providers (vision/OCR) |
| Extractions, chunks, embeddings | Likely | Knowledge, evidence, retrieval | PostgreSQL + object storage | `derived_content` | AI providers (as prompt context) |
| AI raw payloads (optional) | Likely | Debugging, evaluation (consented) | Object storage | `ai_raw_payloads` (≤ 30 d) | — |
| Knowledge, artifacts, exports | Likely | Product output | PostgreSQL + object storage | `knowledge`, `artifacts` | Customer webhooks (IDs only) |
| Audit events | Yes (actor) | Security, accountability | PostgreSQL | `audit` | — |
| Usage events | Low | Billing, cost | PostgreSQL | `usage` | — |
| Telemetry | Pseudonymous IDs only | Operations | Observability backend | 30–90 d | Observability vendor |
| Offline captures | Likely | Offline capture | Device IndexedDB | until sync + purge | — |
| Training/evaluation examples | Pseudonymised | Model improvement (consented) | Evaluation store | dataset lifecycle | Fine-tuning provider (approved only) |

The inventory is reviewed at each gate and when a new module or provider is added.
