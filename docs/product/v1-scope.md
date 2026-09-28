# v1 Scope

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §0.1, §23, §24, §27 |

## Purpose

Separates what is **v1 implemented**, **architecture-ready** (contract and data model exist; feature not built), and **future**. This prevents both over-engineering and later rewrites (BP §27 "Over-engineering before market validation").

## Scope Table

| Capability | v1 implemented | Architecture-ready (not built) | Future |
|---|---|---|---|
| Identity | Magic link / OAuth-OIDC login, sessions, MFA for Owner/Admin | SAML SSO (Enterprise) | SCIM provisioning |
| Organizations | Personal + team orgs, memberships, invitations, teams | Data region binding (`data_region`) | Multi-region deployment |
| Access | System roles, permission evaluator, resource scopes, custom roles (plan-limited, [OD-030](../governance/open-decisions.md#od-030)) | ABAC conditions in policy model | Policy-as-code authoring UI |
| Entitlements | Plan entitlements, org overrides, quotas, module entitlements | Contract-based Enterprise overrides UI | Self-serve add-on marketplace purchases |
| Module Store | Catalog, install, enable/disable, configure, upgrade, version pinning, official modules only (M0) | Partner declarative modules (M1) data flows | Remote modules (M2), public SDK + payouts (M3), ranking (M4) |
| Module runtime | Declarative + official native steps | Remote runtime contract | Third-party execution |
| Ingestion | Audio, Video, Image/Scan, Handwriting, PDF, DOCX, PPTX, XLSX, Text | External source connectors | Live streaming transcription ([OD-036](../governance/open-decisions.md#od-036)) |
| Jobs | Full state machine, retry, cancel, review, SSE | Scheduled/recurring jobs | — |
| AI | Gateway, router, fallback, ≥ 2 adapters, prompt/model registry, eval pipeline | Regional AI routing, private AI policy per tenant (Enterprise) | Fine-tuned/specialised models |
| Knowledge | Entities, relations, evidence, review, corrections, retrieval (tenant-scoped) | Cross-job knowledge graph per project | Organization-wide knowledge search product |
| Artifacts | Versioning, structured editor, templates, brand kit, numbering, PDF/DOCX, approvals | Email/Slides renderers | Real-time co-editing (WebSocket) |
| Workflow | Configurable review/approval chains | Conditional routing rules | Visual workflow designer |
| Actions | Action Items with status/owner/deadline | External task-tool sync | — |
| Billing | Plans, subscriptions, usage metering, quotas, overage calculation, one payment provider ([OD-010](../governance/open-decisions.md#od-010)) | Multiple regional providers | Marketplace payouts |
| Privacy | Retention policies, export, deletion, training consent | Data residency | — |
| Audit | Append-only audit, query UI, export (plan-gated) | SIEM streaming | — |
| Integrations | Public REST (subset), API keys, webhooks | Connectors framework | Zapier-style catalog |
| PWA | Installable, offline capture + upload queue for Site Report | Full offline editing | Native mobile apps |

## Related Documents

[non-goals](non-goals.md), [execution-roadmap](../execution/execution-roadmap.md), [marketplace-roadmap](../marketplace/marketplace-roadmap.md)
