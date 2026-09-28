# Clerk AI — Technical Documentation

| Field | Value |
|---|---|
| Status | Accepted (documentation baseline) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Documentation Lead / Technical Leadership |
| Source of Truth | [Blueprint v1.0 (EN)](source/README.md) |

## What Clerk AI Is

Clerk AI is a **modular AI documentation platform** that transforms unstructured human input — audio, video, images, handwriting, scans, PDF, DOCX, PPTX, XLSX, text — into **structured, traceable, editable, professional artifacts**. Its differentiation is the reusable pipeline:

```text
Unstructured Sources → Understanding → Structured Knowledge → Evidence → Professional Artifact → Workflow
```

Four launch modules — `business.meeting-minutes`, `construction.site-report`, `general.handwriting-document`, `education.lecture-summary` — are built **on top of** a generic platform; the platform is never built around them. Start with the [product overview](product/product-overview.md).

## Source of Truth Hierarchy

1. Accepted ADR/PDR/BDR that explicitly supersede a Blueprint section
2. [Blueprint v1.0 (EN)](source/README.md)
3. Machine-readable contracts ([OpenAPI](api/openapi/openapi.yaml), [JSON Schemas](api/schemas/), [module manifests](modules/manifests/))
4. Canonical specification documents (one owner per concept)
5. Everything else

Details: [source-of-truth-policy](governance/source-of-truth-policy.md). Conventions: [documentation-conventions](governance/documentation-conventions.md). Terms: [glossary](governance/glossary.md).

## Architecture Summary

Go **Modular Monolith** (API + worker processes) · **Nuxt 4 PWA** · **PostgreSQL** (+ RLS, pgvector) · **Redis** (ephemeral) · **S3-compatible storage** (direct presigned uploads) · **Temporal** (durable workflows) · **Transactional Outbox + EventBus port** · **REST + OpenAPI** · **SSE** · **OpenTelemetry** · **provider-neutral AI Gateway** with capability profiles, prompt registry and evaluation · **declarative-first modules** · **multi-tenant from day one**. Read [architecture-overview](architecture/architecture-overview.md).

## Key Decisions

| Area | Records |
|---|---|
| Architecture | [ADR index](governance/adr/README.md) (ADR-001…ADR-027) |
| Product | [PDR index](governance/pdr/README.md) (PDR-001…PDR-008) |
| Business | [BDR index](governance/bdr/README.md) (BDR-001…BDR-005) |
| Open decisions | [open-decisions](governance/open-decisions.md) (OD-001…OD-041; OD-001/002/003/024 closed) |

## Documentation Map by Domain

| Domain | Entry point |
|---|---|
| Governance | [architecture-principles](governance/architecture-principles.md), [product-principles](governance/product-principles.md), [business-principles](governance/business-principles.md), [traceability-matrix](governance/traceability-matrix.md) |
| Product | [product-overview](product/product-overview.md), [v1-scope](product/v1-scope.md), [module-catalog](product/module-catalog.md) |
| Architecture | [architecture-overview](architecture/architecture-overview.md), [bounded-contexts](architecture/bounded-contexts.md), [tenancy-architecture](architecture/tenancy-architecture.md), [event-architecture](architecture/event-architecture.md) |
| Data | [data-model-overview](data/data-model-overview.md), [erd](data/erd.md), [entity-catalog](data/entity-catalog.md), [migration-order](data/migration-order.md) |
| API | [api-principles](api/api-principles.md), [openapi.yaml](api/openapi/openapi.yaml), [errors](api/errors.md), [error-catalog](api/error-catalog.md) |
| Platform services | [platform-core](platform/platform-core.md) (identity, organizations, access, entitlements, projects, actions, collaboration, notifications, audit, flags) |
| Modules | [modules/README](modules/README.md), [module-runtime-contract](modules/module-runtime-contract.md), [manifest schema](api/schemas/module-manifest.schema.json) |
| Ingestion | [universal-ingestion](ingestion/universal-ingestion.md) |
| Jobs | [job-state-machine](jobs/job-state-machine.md), [temporal-workflows](jobs/temporal-workflows.md) |
| Knowledge | [knowledge-layer](knowledge/knowledge-layer.md), [evidence-model](knowledge/evidence-model.md) |
| AI | [ai-architecture](ai/ai-architecture.md), [evaluation](ai/evaluation/evaluation-strategy.md), [training](ai/training/training-strategy.md) |
| AI governance | [ai-governance](governance-ai/ai-governance.md) |
| Artifacts | [artifact-engine](artifacts/artifact-engine.md), [approval-lifecycle](artifacts/approval-lifecycle.md) |
| Billing | [billing-architecture](billing/billing-architecture.md), [pricing-model](billing/pricing-model.md) |
| Security | [security-architecture](security/security-architecture.md), [threat-model](security/threat-model.md) |
| Privacy | [privacy-architecture](privacy/privacy-architecture.md), [data-retention](privacy/data-retention.md) |
| Frontend | [frontend-architecture](frontend/frontend-architecture.md), [information-architecture](frontend/information-architecture.md) |
| PWA / offline | [pwa-architecture](pwa/pwa-architecture.md), [synchronization](pwa/synchronization.md) |
| Marketplace | [marketplace-architecture](marketplace/marketplace-architecture.md) |
| Integrations | [integration-architecture](integrations/integration-architecture.md) |
| Operations | [environments](operations/environments.md), [ci-cd](operations/ci-cd.md), [observability](operations/observability.md), [runbooks](operations/runbooks/README.md) |
| Testing | [testing-strategy](testing/testing-strategy.md), [golden-tests](testing/golden-tests.md) |
| Execution | [execution-roadmap](execution/execution-roadmap.md), [sprint-0](execution/sprint-0.md), [sprint-1](execution/sprint-1.md), [documentation-readiness-report](execution/documentation-readiness-report.md) |

## Launch Modules

| Module | Spec | Manifest |
|---|---|---|
| Meeting Minutes | [meeting-minutes/](modules/meeting-minutes/README.md) | [yaml](modules/manifests/meeting-minutes.example.yaml) |
| Construction Site Report | [site-report/](modules/site-report/README.md) | [yaml](modules/manifests/site-report.example.yaml) |
| Handwriting → Document | [handwriting-document/](modules/handwriting-document/README.md) | [yaml](modules/manifests/handwriting-document.example.yaml) |
| Lecture Summary | [lecture-summary/](modules/lecture-summary/README.md) | [yaml](modules/manifests/lecture-summary.example.yaml) |
| Hello (vertical slice) | [sprint-1](execution/sprint-1.md) | [yaml](modules/manifests/hello.example.yaml) |

## Reading Paths by Audience

| Audience | Read in order |
|---|---|
| **New Engineer** | [product-overview](product/product-overview.md) → [glossary](governance/glossary.md) → [architecture-overview](architecture/architecture-overview.md) → [bounded-contexts](architecture/bounded-contexts.md) → [sprint-0](execution/sprint-0.md) |
| **Backend Engineer** | [component-architecture](architecture/component-architecture.md) → [dependency-rules](architecture/dependency-rules.md) → [tenancy-architecture](architecture/tenancy-architecture.md) → [entity-catalog](data/entity-catalog.md) → [openapi.yaml](api/openapi/openapi.yaml) → [job-state-machine](jobs/job-state-machine.md) → [temporal-workflows](jobs/temporal-workflows.md) → [event-architecture](architecture/event-architecture.md) |
| **Frontend Engineer** | [frontend-architecture](frontend/frontend-architecture.md) → [information-architecture](frontend/information-architecture.md) → [module-ui-contract](modules/module-ui-contract.md) → [editor-contract](artifacts/editor-contract.md) → [pwa-architecture](pwa/pwa-architecture.md) → [error-handling](frontend/error-handling.md) |
| **AI Engineer** | [ai-architecture](ai/ai-architecture.md) → [ai-gateway](ai/ai-gateway.md) → [model-profiles](ai/model-profiles.md) → [prompt-registry](ai/prompt-registry.md) → [knowledge-layer](knowledge/knowledge-layer.md) → [evaluation-strategy](ai/evaluation/evaluation-strategy.md) → module `ai-profile.md` / `evaluation-plan.md` |
| **DevOps / SRE** | [deployment-architecture](architecture/deployment-architecture.md) → [environments](operations/environments.md) → [ci-cd](operations/ci-cd.md) → [observability](operations/observability.md) → [sli-slo](operations/sli-slo.md) → [runbooks](operations/runbooks/README.md) → [disaster-recovery](operations/disaster-recovery.md) |
| **Security Reviewer** | [security-architecture](security/security-architecture.md) → [threat-model](security/threat-model.md) → [tenant-isolation](security/tenant-isolation.md) → [ai-security](security/ai-security.md) → [privacy-architecture](privacy/privacy-architecture.md) → [security-release-gates](security/security-release-gates.md) |
| **Product Manager** | [product-overview](product/product-overview.md) → [personas](product/personas.md) → [v1-scope](product/v1-scope.md) → module `product-spec.md` files → [pricing-model](billing/pricing-model.md) → [execution-roadmap](execution/execution-roadmap.md) |
| **Module Author** | [module-platform](modules/module-platform.md) → [module-authoring-guide](modules/sdk/module-authoring-guide.md) → [manifest-reference](modules/sdk/manifest-reference.md) → [workflow-reference](modules/sdk/workflow-reference.md) → [schema-reference](modules/sdk/schema-reference.md) → [testing-modules](modules/sdk/testing-modules.md) |
| **Technical Leadership** | [architecture-principles](governance/architecture-principles.md) → [ADR index](governance/adr/README.md) → [open-decisions](governance/open-decisions.md) → [risk-register](execution/risk-register.md) → [traceability-matrix](governance/traceability-matrix.md) → [documentation-readiness-report](execution/documentation-readiness-report.md) |

## Execution

[execution-roadmap](execution/execution-roadmap.md) (gates G0–G14) · [implementation-order](execution/implementation-order.md) · [sprint-0](execution/sprint-0.md) · [sprint-1](execution/sprint-1.md) · [implementation-entry-checklist](execution/implementation-entry-checklist.md) · [definition-of-ready](execution/definition-of-ready.md) · [definition-of-done](execution/definition-of-done.md) · [documentation-readiness-report](execution/documentation-readiness-report.md)

## How to Update Documentation

1. Find the **canonical owner** of the concept ([documentation-conventions §4](governance/documentation-conventions.md#4-canonical-ownership)); edit it there and link from elsewhere.
2. Architectural/product/business changes require a new ADR/PDR/BDR; never edit an Accepted record's decision.
3. Resolving an open decision: add the decision record, update [open-decisions](governance/open-decisions.md), update affected documents.
4. Update the [traceability-matrix](governance/traceability-matrix.md) if Blueprint coverage changes.
5. CI validates links, Mermaid diagrams, JSON Schemas, OpenAPI and manifests.
