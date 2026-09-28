# Epic Backlog (v1)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + Tech Lead |
| Blueprint | BP §25 |

| Epic | Scope (BP §25) | Gate | Primary specs |
|---|---|---|---|
| EPIC-01 Identity & Organization | Auth, membership, team, invitation, sessions | G4 | [identity](../platform/identity/authentication.md), [organizations](../platform/organizations/organizations.md) |
| EPIC-02 Access & Policy | RBAC, custom roles, resource scopes, permission guards | G4 | [authorization-model](../platform/access/authorization-model.md) |
| EPIC-03 Commerce Core | Plan, entitlement, usage meters, quotas | G1/G12 | [entitlement-model](../platform/entitlements/entitlement-model.md), [usage-metering](../billing/usage-metering.md) |
| EPIC-04 Module Registry | Manifest validator, versions, catalog | G5 | [module-registry](../modules/module-registry.md), [module-validation](../modules/module-validation.md) |
| EPIC-05 Module Installation | Install/disable/config/dependency/upgrade | G5 | [module-installation](../modules/module-installation.md) |
| EPIC-06 Source Platform | Upload, object storage, metadata, malware scan, dedup | G6 | [upload-flow](../ingestion/upload-flow.md) |
| EPIC-07 Source Processors | Audio/video/image/PDF/office/text extraction | G6 | [universal-ingestion](../ingestion/universal-ingestion.md) |
| EPIC-08 Jobs & Temporal | State machine, workflow, SSE status, retry | G7 | [job-state-machine](../jobs/job-state-machine.md), [temporal-workflows](../jobs/temporal-workflows.md) |
| EPIC-09 AI Gateway | Providers, model router, structured output, cost logging | G8 | [ai-gateway](../ai/ai-gateway.md) |
| EPIC-10 Prompt / Eval | Registry, datasets, regression runner | G8 | [prompt-registry](../ai/prompt-registry.md), [evaluation-strategy](../ai/evaluation/evaluation-strategy.md) |
| EPIC-11 Knowledge | Entities, relations, evidence, corrections | G9 | [knowledge-layer](../knowledge/knowledge-layer.md) |
| EPIC-12 Artifact Engine | Schema, versions, editor contract, renderer | G10 | [artifact-engine](../artifacts/artifact-engine.md) |
| EPIC-13 Templates | Brand Kit, numbering, PDF/DOCX | G10 | [template-engine](../artifacts/template-engine.md) |
| EPIC-14 Workflow / Approval | Review items, approvals, action items | G10 | [approval](../platform/collaboration/approval.md), [review](../platform/collaboration/review.md), [action-items](../platform/actions/action-items.md) |
| EPIC-15 Home & Module Store UX | Cards, discover, installed, launcher | G5/G10 | [module-launcher](../frontend/module-launcher.md), [module-store-ui](../frontend/module-store-ui.md) |
| EPIC-16 Meeting Minutes | Module implementation | G11 | [meeting-minutes](../modules/meeting-minutes/README.md) |
| EPIC-17 Site Report | Module + mobile/offline | G11 | [site-report](../modules/site-report/README.md), [pwa](../pwa/pwa-architecture.md) |
| EPIC-18 Handwriting Document | Module implementation | G11 | [handwriting-document](../modules/handwriting-document/README.md) |
| EPIC-19 Lecture Summary | Module implementation | G11 | [lecture-summary](../modules/lecture-summary/README.md) |
| EPIC-20 Audit / Privacy | Audit, retention, export/delete workflows | G4→G13 | [audit-model](../platform/audit/audit-model.md), [privacy](../privacy/privacy-architecture.md) |
| EPIC-21 Observability / Operations | OTel, dashboards, alerts, backups | Sprint 0→G13 | [observability](../operations/observability.md) |
| EPIC-22 Billing & Launch | Payment provider abstraction, invoices, subscriptions | G12/G14 | [billing-architecture](../billing/billing-architecture.md) |

Each epic requires Definition of Ready before start ([definition-of-ready](definition-of-ready.md)).
