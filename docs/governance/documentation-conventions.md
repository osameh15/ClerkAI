# Documentation Conventions

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Documentation Lead |
| Blueprint | BP §26.1, BP §29 |

## Purpose

Rules that keep ~400 documents coherent: metadata, status vocabulary, structure, canonical ownership, linking, diagrams, and review.

## 1. Language and Style

- Professional English, present tense, active voice.
- Normative keywords follow RFC 2119 semantics when written in capitals: **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, **MAY**.
- No marketing language in technical specifications.
- Domain terms are capitalised when they refer to the platform concept defined in the [glossary](glossary.md) (Source, Job, Artifact, Module, Knowledge, Evidence, Entitlement, Permission). Lowercase when used generically.
- Code identifiers, table names, event types, permission keys and file paths are written in `code` style.

## 2. Metadata Block

Every substantial document starts with a metadata table:

```markdown
| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | <domain or role> |
| Blueprint | BP §x.y |
```

### 2.1 Status (document lifecycle)

| Status | Meaning |
|---|---|
| Draft | Being written; not reviewable. |
| Proposed | Ready for review; not yet binding. |
| Accepted | Binding for implementation. |
| Implemented | Accepted and realised in code; the document now describes production behaviour. |
| Deprecated | Still describes existing behaviour but must not be used for new work. |
| Superseded | Replaced; must name its successor. |

### 2.2 Spec class (maturity of a technical specification)

| Spec class | Meaning |
|---|---|
| Baseline | Principles, invariants and boundaries. Binding, but intentionally not detailed enough to code against alone. |
| Implementation-ready | Detailed enough that an engineer can implement without architectural guesswork. |
| Provisional | Direction is set but values (numbers, thresholds, prices, vendors) are hypotheses to be validated. |
| Future | Architecture space is reserved; implementation is deferred beyond v1. |

### 2.3 Versioning of documents

Semantic versioning: **major** when a contract or invariant changes, **minor** when content is added compatibly, **patch** for editorial fixes. Git history is the change log; no inline change logs.

## 3. Standard Section Order

Use the following headings where applicable, in this order. Omit headings with nothing meaningful to say; never fill them with boilerplate.

1. Purpose
2. Scope / Non-goals
3. Dependencies
4. Definitions (link to the glossary; only local terms here)
5. Architecture / Design
6. Invariants
7. Interfaces / Contracts
8. State Model
9. Failure Modes
10. Security Considerations
11. Privacy Considerations
12. Observability
13. Testing Strategy
14. Operational Considerations
15. Open Questions (each links to an `OD-xxx` in [open-decisions.md](open-decisions.md))
16. Related ADR / PDR / BDR
17. Related Documents

## 4. Canonical Ownership

Each concept has exactly **one** owning document. Other documents link to it and do not restate its definition. Canonical owners of the most contested concepts:

| Concept | Canonical owner |
|---|---|
| Terminology | [glossary.md](glossary.md) |
| Job states and transitions | [jobs/job-state-machine.md](../jobs/job-state-machine.md) |
| ProcessingRun / ProcessingStep | [jobs/processing-runs.md](../jobs/processing-runs.md) |
| Source / SourceFile / Extraction / Chunk | [ingestion/source-model.md](../ingestion/source-model.md) |
| Upload session lifecycle | [ingestion/upload-flow.md](../ingestion/upload-flow.md) |
| Knowledge entity/relation model | [knowledge/entity-model.md](../knowledge/entity-model.md), [knowledge/relation-model.md](../knowledge/relation-model.md) |
| Evidence pointer model | [knowledge/evidence-model.md](../knowledge/evidence-model.md) and [evidence.schema.json](../api/schemas/evidence.schema.json) |
| Confidence bands | [knowledge/confidence-model.md](../knowledge/confidence-model.md) |
| Artifact status lifecycle | [artifacts/approval-lifecycle.md](../artifacts/approval-lifecycle.md) |
| Artifact versioning | [artifacts/artifact-versioning.md](../artifacts/artifact-versioning.md) |
| Module manifest | [module-manifest.schema.json](../api/schemas/module-manifest.schema.json), [modules/sdk/manifest-reference.md](../modules/sdk/manifest-reference.md) |
| Module installation states | [modules/module-lifecycle.md](../modules/module-lifecycle.md) |
| Permission vs Entitlement evaluation | [platform/access/authorization-model.md](../platform/access/authorization-model.md) |
| Permission key namespace | [platform/access/permission-namespaces.md](../platform/access/permission-namespaces.md) |
| Entitlement model | [platform/entitlements/entitlement-model.md](../platform/entitlements/entitlement-model.md) |
| Event envelope and naming | [architecture/event-architecture.md](../architecture/event-architecture.md) and [event-envelope.schema.json](../api/schemas/event-envelope.schema.json) |
| Error contract | [api/errors.md](../api/errors.md), [api/error-catalog.md](../api/error-catalog.md) |
| Idempotency | [architecture/idempotency-strategy.md](../architecture/idempotency-strategy.md) (system), [api/idempotency.md](../api/idempotency.md) (HTTP contract) |
| Tenant isolation | [architecture/tenancy-architecture.md](../architecture/tenancy-architecture.md) |
| Identifiers | [data/identifier-strategy.md](../data/identifier-strategy.md) |
| Usage meters | [billing/usage-event-catalog.md](../billing/usage-event-catalog.md) |
| Audit event catalogue | [platform/audit/audit-event-catalog.md](../platform/audit/audit-event-catalog.md) |
| Data classification | [privacy/data-classification.md](../privacy/data-classification.md) |
| Retention categories | [privacy/data-retention.md](../privacy/data-retention.md) |
| Threat model | [security/threat-model.md](../security/threat-model.md) |
| AI capability profiles | [ai/model-profiles.md](../ai/model-profiles.md) |
| Prompt identity and versions | [ai/prompt-registry.md](../ai/prompt-registry.md) |
| Gates G0–G14 | [execution/execution-roadmap.md](../execution/execution-roadmap.md) |
| Open decisions | [open-decisions.md](open-decisions.md) |

## 5. Linking

- Relative Markdown links only. No absolute URLs to the repository.
- Link to the canonical owner on the first mention of a concept in each document.
- Decision records are referenced by ID and linked: `[ADR-007](adr/ADR-007-event-architecture.md)`.
- Open decisions are referenced as `OD-xxx` and linked to their anchor in [open-decisions.md](open-decisions.md).
- Blueprint references use `BP §n.n` or `BP App. X`.

## 6. Diagrams

- Mermaid only, embedded in fenced ` ```mermaid ` blocks, so diagrams are diffable and render in common Git hosts.
- Every diagram must communicate a decision (boundary, dependency direction, state transition, sequence, ownership). Decorative diagrams are not allowed.
- Diagrams must parse with the Mermaid CLI; CI runs the check (see [ci-cd.md](../operations/ci-cd.md)).
- Prefer `flowchart`, `sequenceDiagram`, `stateDiagram-v2`, `erDiagram`.

## 7. TBD Handling

When a document cannot yet be completed:

1. State the confirmed decisions and invariants.
2. Add a `TBD` marker that names the missing decision.
3. Link the `TBD` to an `OD-xxx` entry.

`TBD` without an `OD` reference is a documentation defect.

## 8. File Naming

- `kebab-case.md` for documents.
- Decision records: `ADR-NNN-short-title.md`, `PDR-NNN-...`, `BDR-NNN-...`. Numbers are never reused.
- Machine-readable contracts: `*.schema.json` (JSON Schema 2020-12), `openapi.yaml` (OpenAPI 3.1), module manifests `*.yaml`.

## 9. Review and Quality Gates for Documentation

A documentation pull request is mergeable when:

- Metadata block present and status correct.
- All relative links resolve (CI link check).
- Mermaid blocks parse; JSON Schemas compile; OpenAPI validates; example manifests validate against the manifest schema.
- The traceability matrix is updated if Blueprint coverage changed.
- No definition is duplicated from its canonical owner.

## Related Documents

- [source-of-truth-policy.md](source-of-truth-policy.md)
- [glossary.md](glossary.md)
- [adr/README.md](adr/README.md)
