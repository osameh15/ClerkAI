# Platform Core

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Technical Leadership |
| Blueprint | BP §0.1, §1.1, §7 |

## Definition

**Platform Core** is every bounded context that exists independently of any Module: identity, organizations, access, commerce (plans, entitlements, usage, billing), module platform, projects, sources/ingestion, jobs/processing, AI, evaluation, knowledge, artifacts, templates, workflow, actions, collaboration, notifications, integrations, audit, privacy. See [bounded-contexts](../architecture/bounded-contexts.md).

## The Platform Test

Platform Core is correct when a new module (for example HSE Report, Interview Summary, Board Minutes, Exam Builder) can be added by publishing a module package — manifest, schemas, prompts, workflow definition, templates, evaluation suite — **without changing** Authentication, Organizations, Permissions, Entitlements, Uploads, AI Gateway, Knowledge, Evidence, Artifact Engine, Workflow, Billing, Audit or Module Store. The Hello Module in Sprint 0/1 is the first executable proof ([sprint-1](../execution/sprint-1.md)).

## What Platform Core Provides to Modules

| Capability | Provided by | Module declares |
|---|---|---|
| Tenancy, identity, sessions | identity, organization | nothing |
| Permission registration and checks | access | `permissions[]` |
| Entitlement gating and metering | commerce | `entitlements.required`, `features`, `meters` |
| Source capture, ingestion, extraction, chunking | source | `inputs.slots[].requiredExtractions` |
| Durable processing | processing (Temporal) | `workflows.processing` (step DAG) |
| AI capabilities with routing/fallback/validation | ai | `ai.tasks` (profiles, prompts, schemas) |
| Knowledge graph, evidence, review | knowledge | `knowledge.entityTypes`, `relationTypes`, thresholds |
| Structured artifacts, versions, editor | artifact | `artifacts[]` schemas |
| Templates, brand kit, numbering, rendering | template | `artifacts[].templates`, `numbering` |
| Review/approval workflows | workflow | `workflows.approval` |
| Action tracking | action | `artifacts[].createsActionItems` |
| Audit, usage, observability | audit, commerce, telemetry | `telemetry` |
| Store, install, config, versioning | module | manifest itself |

## Platform Subdomains Documented Here

[identity](identity/authentication.md) · [organizations](organizations/organizations.md) · [access](access/authorization-model.md) · [entitlements](entitlements/entitlement-model.md) · [projects](projects/project-domain.md) · [actions](actions/action-items.md) · [collaboration](collaboration/review.md) · [notifications](notifications/notification-architecture.md) · [audit](audit/audit-model.md) · [feature flags](feature-flags/feature-flag-strategy.md)
