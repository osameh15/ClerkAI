# Architecture Principles

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Technical Leadership |
| Blueprint | BP §1.1, §6, §7.2, §28.1 |

## Purpose

The binding principles and invariants that every design, specification and pull request must respect. Each principle states the rule, the reason, how it is enforced, and what a violation looks like.

## Principles

### AP-01 Module-first
- **Rule:** No Site Report, Meeting, Education or other domain-specific behaviour is hard-coded into Platform Core. Modules extend the platform through the [Module Runtime Contract](../modules/module-runtime-contract.md).
- **Why:** A new module (for example HSE Report) must be addable without changing Authentication, Uploads, AI Gateway, Knowledge, Artifacts, Workflow, Billing, Audit or Module Store.
- **Enforcement:** `internal/` packages may not import `modules/`; CI grep for module keys in core packages; architecture review.
- **Violation example:** `if module == "construction.site-report"` in the Jobs context.

### AP-02 Source-first
- **Rule:** Sources are tenant-owned, immutable and independent of Modules; one Source can feed many Jobs and Artifacts ([PDR-004](pdr/PDR-004-source-reusability.md)).
- **Enforcement:** `sources` has no `module_id` or `job_id` column; the link is `job_sources`.

### AP-03 Knowledge-first
- **Rule:** AI produces structured Knowledge first; Artifacts are derived views of Knowledge.
- **Enforcement:** Artifact generation activities consume Knowledge, not raw model output ([ADR-010](adr/ADR-010-structured-ai-output.md)).

### AP-04 Evidence-first
- **Rule:** Important extracted facts link to Source locations whenever technically possible ([ADR-011](adr/ADR-011-evidence-first.md)).
- **Enforcement:** Knowledge schemas declare which fields require Evidence; missing Evidence produces a Review Item rather than silent acceptance.

### AP-05 Human-in-the-loop
- **Rule:** Low-confidence or high-risk output is reviewable, editable, confirmable or rejectable. Nothing AI-generated becomes Final without passing the module's review/approval policy.

### AP-06 Multi-tenant from Day One
- **Rule:** Every tenant-owned row carries `organization_id`; application guard + PostgreSQL RLS on sensitive tables ([ADR-013](adr/ADR-013-multi-tenant-isolation.md)). Tenant context propagates into workers, events, cache keys and object keys.

### AP-07 Entitlement-aware
- **Rule:** Authenticated → Tenant access → Entitled → Permitted → Resource Policy → Allowed ([authorization-model](../platform/access/authorization-model.md)). Permission alone is never enough.

### AP-08 Version everything important
- **Rule:** Modules, manifests, module schemas, prompts, model profiles, templates, workflows, evaluation datasets, artifacts, APIs and events are versioned. Published versions are immutable.

### AP-09 Vendor-neutral AI
- **Rule:** Modules request capabilities via Model Profiles, never vendors or model names ([ADR-017](adr/ADR-017-provider-neutral-ai.md)).

### AP-10 Audit by default
- **Rule:** AI generation, human edits, approvals, exports, role/module/billing changes are recorded as append-only Audit Events distinguishing AI, Human, and System actors.

### AP-11 Scale by extraction, never by rewrite
- **Rule:** Go Modular Monolith with enforced bounded contexts ([ADR-002](adr/ADR-002-modular-monolith.md)). A context or worker pool can be extracted into a service without changing its domain model. No premature microservices.

### AP-12 Privacy by design
- **Rule:** Data minimisation, configurable retention per data category, and no use of customer content for general model training by default.

### AP-13 PostgreSQL is the system of record
- **Rule:** Business state lives in PostgreSQL ([ADR-006](adr/ADR-006-postgresql-primary-store.md)). Temporal holds execution state; the Event Bus carries notifications; Redis holds ephemeral state; Object Storage holds binaries. None of these is authoritative for business facts.

### AP-14 Contracts before code
- **Rule:** OpenAPI, JSON Schemas, event schemas and module manifests are authored and reviewed before implementation, and validated in CI ([ADR-004](adr/ADR-004-rest-openapi.md)).

### AP-15 Idempotent commands, at-least-once everything
- **Rule:** Critical commands accept `Idempotency-Key`; every event consumer and Temporal activity is idempotent ([ADR-012](adr/ADR-012-command-idempotency.md)).

### AP-16 Untrusted input by default
- **Rule:** Uploaded content, model output and third-party module output are untrusted. Source content is never an instruction.

### AP-17 Binaries never through the API
- **Rule:** Large binaries are transferred directly between client and Object Storage via presigned URLs ([ADR-005](adr/ADR-005-direct-object-storage-upload.md)); PostgreSQL stores metadata and references only.

### AP-18 Structured content is the source of truth
- **Rule:** Artifact content is schema-versioned JSON; HTML/PDF/DOCX are renderings ([ADR-016](adr/ADR-016-structured-artifact-content.md)).

## Architectural Invariants (testable)

| ID | Invariant | Verified by |
|---|---|---|
| INV-01 | No query on a tenant-owned table executes without a tenant context (except audited platform-admin paths). | Repository tests, RLS tests ([tenant-data-isolation](../data/tenant-data-isolation.md)) |
| INV-02 | Original SourceFiles are never overwritten or mutated. | Object-storage policy + domain tests |
| INV-03 | ArtifactVersions are append-only. | DB trigger + domain tests |
| INV-04 | `audit_events` and `usage_events` accept INSERT only from the application role. | DB grants + tests |
| INV-05 | Every AI call produces exactly one AIExecution per attempt. | AI Gateway contract tests |
| INV-06 | A Job's Module version and workflow version are pinned at submit and never change for that ProcessingRun. | Job domain tests |
| INV-07 | Platform Core packages contain no module key literals. | CI static check |
| INV-08 | A domain never reads another domain's tables. | Import/SQL boundary lint ([dependency-rules](../architecture/dependency-rules.md)) |
| INV-09 | Every outbound event carries `organization_id` (or is explicitly platform-scoped) and `correlation_id`. | Event schema validation |
| INV-10 | Disable/Uninstall never deletes Artifacts. | Module lifecycle tests |

## Related Documents

- [product-principles.md](product-principles.md), [business-principles.md](business-principles.md)
- [architecture-overview.md](../architecture/architecture-overview.md)
- [dependency-rules.md](../architecture/dependency-rules.md)
