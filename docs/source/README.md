# Source Documents

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Documentation Lead |

## Purpose

This directory holds the authoritative product and architecture source documents from which every other document in `/docs` is derived.

## Contents

| File | Role | Authority |
|---|---|---|
| `Clerk_AI_Blueprint_EN.docx` | Blueprint v1.0, "Execution Baseline", dated Mehr 1405 / September 2026 | **Primary Source of Truth** |
| `Clerk_AI_Blueprint_FA.docx` | Persian edition of the same Blueprint | Reference translation. Section structure verified identical to the English edition (sections 0–30 and Appendices A–F). Not authoritative where wording differs. |

## Rules

1. The English Blueprint wins over every other document unless a newer, **Accepted** ADR/PDR/BDR explicitly supersedes a specific decision. See [source-of-truth-policy.md](../governance/source-of-truth-policy.md).
2. The `.docx` files are never edited in place. A new Blueprint revision is added as a new file (for example `Clerk_AI_Blueprint_EN_v1.1.docx`) and the change is recorded in the relevant decision log.
3. Blueprint section references in `/docs` use the form `BP §<n>` (for example `BP §8.1`) or `BP App. <X>` for appendices.
4. The [traceability matrix](../governance/traceability-matrix.md) maps every Blueprint section to the documents that elaborate it.

## Blueprint Section Map (quick reference)

| BP § | Title | Primary elaborating area |
|---|---|---|
| 0 | Executive Summary | [README](../README.md), [product-overview](../product/product-overview.md) |
| 1 | Product Vision and Foundational Principles | [architecture-principles](../governance/architecture-principles.md), [product-principles](../governance/product-principles.md) |
| 2 | Users, Market, Core Scenarios | [personas](../product/personas.md), [product-kpis](../product/product-kpis.md) |
| 3 | Four Launch Modules | [modules/](../modules/README.md) |
| 4 | Module Store and Module Economics | [module-store](../modules/module-store.md), [module-lifecycle](../modules/module-lifecycle.md) |
| 5 | Revenue Model and Pricing | [billing/pricing-model](../billing/pricing-model.md) |
| 6 | High-Level Technical Architecture | [architecture-overview](../architecture/architecture-overview.md) |
| 7 | Domain Architecture and Data Model | [bounded-contexts](../architecture/bounded-contexts.md), [data/](../data/data-model-overview.md) |
| 8 | API, Runtime, Event Architecture | [api/](../api/api-principles.md), [event-architecture](../architecture/event-architecture.md), [job-state-machine](../jobs/job-state-machine.md) |
| 9 | Universal Ingestion | [ingestion/](../ingestion/universal-ingestion.md) |
| 10 | Knowledge Layer and Evidence | [knowledge/](../knowledge/knowledge-layer.md) |
| 11 | AI Architecture, Training, Evaluation | [ai/](../ai/ai-architecture.md) |
| 12 | Artifact, Template, Workflow | [artifacts/](../artifacts/artifact-engine.md) |
| 13 | Access Control, Permissions, Entitlements | [platform/access](../platform/access/authorization-model.md), [platform/entitlements](../platform/entitlements/entitlement-model.md) |
| 14 | Security Architecture | [security/](../security/security-architecture.md) |
| 15 | Privacy and Data Governance | [privacy/](../privacy/privacy-architecture.md) |
| 16 | AI Governance and Trust | [governance-ai/](../governance-ai/ai-governance.md) |
| 17 | Observability, SRE, Operations | [operations/observability](../operations/observability.md) |
| 18 | DevOps, CI/CD, Environments | [operations/ci-cd](../operations/ci-cd.md), [operations/environments](../operations/environments.md) |
| 19 | Testing Strategy and Quality Gates | [testing/](../testing/testing-strategy.md) |
| 20 | Product Design, UX, Design System | [frontend/](../frontend/frontend-architecture.md) |
| 21 | PWA, Offline, Mobile-first | [pwa/](../pwa/pwa-architecture.md) |
| 22 | Billing, Metering, Unit Economics | [billing/](../billing/billing-architecture.md) |
| 23 | Marketplace and Third-Party Modules | [marketplace/](../marketplace/marketplace-architecture.md) |
| 24 | Execution Roadmap and Gates | [execution-roadmap](../execution/execution-roadmap.md) |
| 25 | Epic-Level Backlog | [epic-backlog](../execution/epic-backlog.md) |
| 26 | Team, Roles, Delivery Model | [ownership-raci](../execution/ownership-raci.md) |
| 27 | Risks and Mitigation | [risk-register](../execution/risk-register.md) |
| 28 | Initial ADR/PDR/BDR | [adr/](../governance/adr/README.md), [pdr/](../governance/pdr/README.md), [bdr/](../governance/bdr/README.md) |
| 29 | DoR / DoD / Entry to Execution | [definition-of-ready](../execution/definition-of-ready.md), [definition-of-done](../execution/definition-of-done.md), [sprint-0](../execution/sprint-0.md) |
| 30 | Reference Standards, Glossary | [glossary](../governance/glossary.md) |
| App. A | Screen Inventory and User Flows | [information-architecture](../frontend/information-architecture.md) |
| App. B | API v1 Inventory | [openapi.yaml](../api/openapi/openapi.yaml) |
| App. C | Permission and Entitlement Matrix | [permission-matrix](../platform/access/permission-matrix.md), [plan-entitlements](../platform/entitlements/plan-entitlements.md) |
| App. D | Acceptance Criteria and AI Quality Targets | [evaluation-metrics](../ai/evaluation/evaluation-metrics.md), module `acceptance-criteria.md` files |
| App. E | Migration Order and Baseline Schema | [migration-order](../data/migration-order.md), [indexing-strategy](../data/indexing-strategy.md) |
| App. F | Sprint 0, Sprint 1, Entry Checklist | [sprint-0](../execution/sprint-0.md), [sprint-1](../execution/sprint-1.md), [implementation-entry-checklist](../execution/implementation-entry-checklist.md) |
