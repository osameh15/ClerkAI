# Glossary

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Documentation Lead / Architecture |
| Blueprint | BP §30.1 (extended) |

## Purpose

Canonical definitions of platform terms. When any other document uses one of these terms it uses this meaning. Terms marked **(BP)** come directly from Blueprint §30.1. Pairs that are commonly confused are disambiguated in [§ Disambiguation](#disambiguation).

## Terms

| Term | Definition | Owner doc |
|---|---|---|
| **Action Item** | A trackable commitment (title, owner, deadline, status) derived from Knowledge or created manually; lives independently of the Artifact that produced it. | [action-items.md](../platform/actions/action-items.md) |
| **ADR / PDR / BDR** (BP) | Architecture / Product / Business Decision Record. | [adr/README.md](adr/README.md) |
| **AI Capability** | An abstract kind of AI work a Module can request (for example `transcription`, `structured_extraction`, `vision_ocr`). | [model-profiles.md](../ai/model-profiles.md) |
| **AI Credit** | Customer-facing consumption unit for AI work. Converted from raw usage by the Pricing Engine. Never equal to a token. | [ai-credit-model.md](../billing/ai-credit-model.md) |
| **AIExecution** (BP) | Immutable record of one model call attempt, including provider, model, prompt version, schema version, usage, cost, latency, outcome. Every fallback attempt is a separate AIExecution. | [ai-execution-model.md](../ai/ai-execution-model.md) |
| **AI Gateway** | Platform component that accepts capability requests, routes to a provider/model, validates output and records AIExecutions. | [ai-gateway.md](../ai/ai-gateway.md) |
| **Approval Request** | A request for one or more decisions on an Artifact version within a Workflow Instance. | [approval.md](../platform/collaboration/approval.md) |
| **Artifact** (BP) | A logical, usable output (for example Minutes, Site Report). Its content is always held in versions. | [artifact-domain.md](../artifacts/artifact-domain.md) |
| **ArtifactVersion** | Immutable, schema-versioned JSON snapshot of an Artifact's content. Every AI or human edit creates a new one. | [artifact-versioning.md](../artifacts/artifact-versioning.md) |
| **Artifact Export** | An asynchronous rendering of a specific ArtifactVersion to a format (PDF, DOCX) stored in Object Storage. | [rendering.md](../artifacts/rendering.md) |
| **Audit Event** | Append-only record of a security-, business- or governance-relevant action and its actor. Distinct from telemetry. | [audit-model.md](../platform/audit/audit-model.md) |
| **Brand Kit** | Organization-level presentation assets: logo, fonts, colours, header/footer, signatures. | [brand-kit.md](../artifacts/brand-kit.md) |
| **Capability Profile / Model Profile** | Named abstract alias (for example `persian_transcription`) that the router resolves to a concrete model given policy. Modules reference profiles, never models. | [model-profiles.md](../ai/model-profiles.md) |
| **Chunk (Source Chunk)** | A bounded, addressable segment of Normalized Content (text span, time range, page region) used for retrieval, prompting and Evidence. | [chunking.md](../ingestion/chunking.md) |
| **Confidence** | A score in [0,1] attached to extracted Knowledge. Raw confidence is not assumed calibrated. | [confidence-model.md](../knowledge/confidence-model.md) |
| **Context Fusion** | Combining Knowledge and content from multiple Sources of one Job into one consistent Knowledge graph. | [context-fusion.md](../knowledge/context-fusion.md) |
| **Correlation ID** | Identifier that follows one end-to-end business operation across requests, workflows, AI calls and events. | [observability.md](../operations/observability.md) |
| **Entitlement** (BP) | The organization's right to use a capability, derived from Plan, add-ons, contract or override. | [entitlement-model.md](../platform/entitlements/entitlement-model.md) |
| **Evaluation** (BP) | Controlled measurement of model/prompt/schema quality against a versioned dataset. | [evaluation-strategy.md](../ai/evaluation/evaluation-strategy.md) |
| **Event (Domain Event)** | Immutable fact published through the Transactional Outbox using the standard envelope. | [event-architecture.md](../architecture/event-architecture.md) |
| **Evidence** (BP) | Structured, exact pointer from a Knowledge item (or Artifact field) back to a Source location: time range, page, bounding box, text span or chunk. | [evidence-model.md](../knowledge/evidence-model.md) |
| **Extraction (Source Extraction)** | The versioned output of one processing pipeline over one Source (for example a transcript, OCR result). Reusable across Jobs. | [source-model.md](../ingestion/source-model.md) |
| **Feature Flag** | Runtime/deployment control over code paths (global, organization, user, percentage). Not a commercial right. | [feature-flag-strategy.md](../platform/feature-flags/feature-flag-strategy.md) |
| **Job** (BP) | One Clerk execution using a pinned Module version and a set of Sources. First-class domain with its own state machine. | [job-domain.md](../jobs/job-domain.md) |
| **Knowledge** (BP) | Structured extracted data and relationships (entities, relations, evidence links, review states). | [knowledge-layer.md](../knowledge/knowledge-layer.md) |
| **Knowledge Entity** | Typed node in the Knowledge graph (Person, Decision, Action, Observation, Concept, …). | [entity-model.md](../knowledge/entity-model.md) |
| **Knowledge Relation** | Typed edge between two Knowledge Entities (for example `RESPONSIBLE_FOR`, `DUE_AT`). | [relation-model.md](../knowledge/relation-model.md) |
| **Membership** | Relationship between a User and an Organization, carrying roles. | [memberships.md](../platform/organizations/memberships.md) |
| **Module** (BP) | Package defining behaviour, schemas, workflows, prompts, templates and artifacts for a use case. Identified by a stable key such as `business.meeting-minutes`. | [module-platform.md](../modules/module-platform.md) |
| **Module Installation** | An organization's installed instance of a Module, pinned to a ModuleVersion, with configuration and status. | [module-installation.md](../modules/module-installation.md) |
| **Module Manifest** | Declarative, schema-validated description of a ModuleVersion. | [manifest-reference.md](../modules/sdk/manifest-reference.md) |
| **Module Store** | Top-level product surface and platform service for discovering, installing, configuring and managing Modules. | [module-store.md](../modules/module-store.md) |
| **ModuleVersion** | Immutable, semantically-versioned release of a Module. | [module-versioning.md](../modules/module-versioning.md) |
| **Normalized Content** | Source-type-independent representation (text blocks, time-aligned segments, layout regions) produced by ingestion. | [normalization.md](../ingestion/normalization.md) |
| **Organization** | The tenant. All business data belongs to exactly one Organization. A Personal Workspace is a one-member Organization. | [organizations.md](../platform/organizations/organizations.md) |
| **Outbox** | Table written in the same transaction as a state change, from which events are dispatched. | [event-architecture.md](../architecture/event-architecture.md) |
| **Permission** (BP) | Operational right granted to a User through a Role (for example `artifacts.export`). | [authorization-model.md](../platform/access/authorization-model.md) |
| **Plan / PlanVersion** | Commercial offering and an immutable versioned definition of its entitlements, quotas and prices. | [plan-model.md](../billing/plan-model.md) |
| **Platform Core** | All platform bounded contexts that are independent of any specific Module. | [platform-core.md](../platform/platform-core.md) |
| **ProcessingRun** | One execution attempt of a Job's processing (initial submit, retry, reprocess). A Job has 1..n runs; at most one active. | [processing-runs.md](../jobs/processing-runs.md) |
| **ProcessingStep** | A recorded unit of work inside a ProcessingRun (maps to one or more Temporal activities). | [processing-steps.md](../jobs/processing-steps.md) |
| **Project** | Optional organizational context grouping Jobs, Artifacts, Actions and members. | [project-domain.md](../platform/projects/project-domain.md) |
| **Prompt / PromptVersion** | Named prompt identity and its immutable released versions with input/output schemas. Referenced as `key@semver`. | [prompt-registry.md](../ai/prompt-registry.md) |
| **Publisher** | Entity that owns and publishes Modules. In v1 only `clerk-official`. | [publishers.md](../marketplace/publishers.md) |
| **Resource Policy** | Scope rules (team, project, ownership, sharing) evaluated after Permission. | [resource-policies.md](../platform/access/resource-policies.md) |
| **Review Item** | A Knowledge item or Artifact field that requires a human decision (confirm/edit/reject). | [review.md](../platform/collaboration/review.md) |
| **Role** | Named set of Permissions. System roles: Owner, Admin, Manager, Member, Viewer, Guest. | [default-roles.md](../platform/access/default-roles.md) |
| **Runtime Type** | How a Module executes: `declarative`, `native`, `remote`. | [module-runtime-contract.md](../modules/module-runtime-contract.md) |
| **Source** (BP) | Any raw or logical input (Audio, Image, PDF, text…). Tenant-owned, immutable, reusable across Jobs and Modules. | [source-model.md](../ingestion/source-model.md) |
| **SourceFile** | A binary object in Object Storage belonging to a Source: the original or a derived rendition. | [source-model.md](../ingestion/source-model.md) |
| **Template / TemplateVersion** | Layout definition independent of content, versioned. | [template-engine.md](../artifacts/template-engine.md) |
| **Tenant Context** | The authenticated Organization scope carried by every request, workflow, job, event and cache key. | [tenancy-architecture.md](../architecture/tenancy-architecture.md) |
| **Training Example** | A curated, consented, reviewed and optionally anonymised record eligible for evaluation or training datasets. | [training-example-lifecycle.md](../ai/training/training-example-lifecycle.md) |
| **Usage Event** | Append-only record of raw consumption (seconds, pages, tokens, bytes) with attributed provider cost. | [usage-metering.md](../billing/usage-metering.md) |
| **Workflow** (BP) | Durable sequence for processing, review or approval. Two senses — see disambiguation. | [workflow-architecture.md](../architecture/workflow-architecture.md) |
| **Workflow Definition / Version / Instance** | Business workflow (for example approval chain) definition, immutable version, and a running instance bound to a resource. | [workflow-architecture.md](../architecture/workflow-architecture.md) |

## Disambiguation

| Pair | Distinction |
|---|---|
| **Permission vs Entitlement** | Permission: *may this user* do X (RBAC + policy). Entitlement: *has this organization paid for / been granted* X (plan, add-on, contract). Both must pass. Example — Permission: user may export DOCX. Entitlement: organization's plan includes DOCX export. |
| **Entitlement vs Feature Flag** | Entitlement is commercial and durable, evaluated per organization. Feature Flag is operational and temporary, used for rollout and kill switches. A flag never grants a paid capability. |
| **Job vs ProcessingRun** | Job is the user-visible unit of intent and outcome. ProcessingRun is one attempt to execute it. Retries and reprocessing add runs; they do not create new Jobs. |
| **Source vs SourceFile** | Source is the logical, tenant-owned input with metadata and extractions. SourceFile is a physical object (original or derived rendition) in storage. |
| **Source vs Extraction** | Source is immutable input. Extraction is a derived, versioned, recomputable result of a pipeline over that input. |
| **Knowledge vs Artifact** | Knowledge is the structured truth extracted and reviewed from Sources. Artifact is a formatted, versioned view of Knowledge for a purpose. Editing an Artifact does not silently rewrite Knowledge. |
| **Template vs Artifact** | Template is layout. Artifact is content. Rendering combines ArtifactVersion + TemplateVersion + Brand Kit. |
| **Module vs ModuleVersion vs Module Installation** | Module is the stable identity (`key`). ModuleVersion is an immutable release. Installation is one organization's pinned use of a version. |
| **Disable vs Uninstall** | Disable blocks new Jobs; everything else stays. Uninstall logically removes the installation; historical Artifacts remain per retention policy. Neither deletes Artifacts. |
| **Workflow (Temporal) vs Workflow (business)** | Temporal workflow: durable execution mechanism for processing. Business Workflow: review/approval process defined by Workflow Definitions and persisted as Workflow Instances in PostgreSQL. A business workflow may be *executed* by a Temporal workflow, but its state of record is PostgreSQL. |
| **Workflow state vs Artifact state** | Workflow Instance state tracks the approval process. Artifact status is the document's lifecycle, updated by the workflow's outcomes. |
| **Event Bus vs Temporal** | Event Bus carries notifications of facts to interested consumers (fire-and-forget, at-least-once). Temporal orchestrates multi-step work with durable state. Neither is the system of record. |
| **AI Model vs Model Profile** | Model: concrete provider model with pricing/limits. Model Profile: abstract, policy-bearing alias that Modules reference. |
| **Prompt vs PromptVersion** | Prompt is the stable identity (`meeting.action-extraction`). PromptVersion is an immutable release (`@2.3.1`). |
| **Audit vs Observability** | Audit is an append-only, business/security record with retention obligations, queryable by customers. Observability (logs/metrics/traces) is operational telemetry, sampled, short-lived, never a compliance record. |
| **Review vs Approval** | Review resolves AI uncertainty on individual Knowledge items/fields. Approval is a formal sign-off on a whole ArtifactVersion. |
| **Confidence vs Evidence** | Confidence is a (possibly uncalibrated) score of correctness. Evidence is a verifiable pointer to supporting source material. High confidence without Evidence is still unverified. |

## Related Documents

- [product/terminology.md](../product/terminology.md) — user-facing vocabulary and localisation.
- [documentation-conventions.md](documentation-conventions.md)
