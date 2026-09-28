# Source of Truth Policy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Technical Leadership / Documentation Lead |
| Blueprint | BP §28, BP Conclusion |

## Purpose

Defines which artifact wins when two documents disagree, how decisions change, and how documentation stays aligned with implementation.

## Authority Hierarchy

Highest authority first:

1. **Accepted decision records** (ADR / PDR / BDR) that *explicitly* name the Blueprint section they supersede. Silence is not supersession.
2. **Blueprint v1.0 (English)** — [`source/Clerk_AI_Blueprint_EN.docx`](../source/README.md).
3. **Machine-readable contracts** derived from the Blueprint: [`openapi.yaml`](../api/openapi/openapi.yaml), [`api/schemas/*.schema.json`](../api/schemas/), module manifests. When a contract and prose disagree, the contract is presumed correct and the prose is fixed, unless the contract contradicts (1) or (2).
4. **Canonical specification documents** in `/docs`. Each concept has exactly one owning document; see [documentation-conventions.md](documentation-conventions.md#4-canonical-ownership).
5. **Non-canonical documents** that reference a concept. They must link to the owner, not redefine the concept.
6. Code comments, tickets, chat, meeting notes — never authoritative.

The Persian Blueprint is a reference translation only.

## Rules

| # | Rule |
|---|---|
| SOT-1 | No document may silently override the Blueprint. A deviation must be (a) recorded as a new ADR/PDR/BDR with status `Proposed`, or (b) listed in [open-decisions.md](open-decisions.md) until accepted. |
| SOT-2 | Where this documentation *refines* the Blueprint (adds detail without contradicting it), the refinement is marked as such in the owning document and does not require a decision record. |
| SOT-3 | Where the Blueprint is silent, a documented default may be proposed; it must be labelled `Proposed` and, if material, recorded in [open-decisions.md](open-decisions.md). |
| SOT-4 | Where the task brief used to generate this documentation conflicts with the Blueprint, the Blueprint wins. Each such conflict is listed below. |
| SOT-5 | Implementation that diverges from an Accepted spec is a defect in either the code or the spec. It is recorded in the [risk-register](../execution/risk-register.md) and resolved by changing one of them. |

## Recorded Brief-vs-Blueprint Reconciliations

The documentation brief proposed file names and numbering that differ from the Blueprint. Resolution (Blueprint wins):

| Topic | Brief proposal | Blueprint | Resolution |
|---|---|---|---|
| ADR numbering | ADR-006 = events, ADR-013 = provider-neutral AI, ADR-014 = PostgreSQL | ADR-006 = PostgreSQL, ADR-007 = Events, ADR-013 = Tenant isolation, ADR-014 = OpenTelemetry, ADR-015 = S3, ADR-016 = Structured artifact content | Blueprint IDs kept. Provider-neutral AI added as **ADR-017**. Mapping in [adr/README.md](adr/README.md). |
| PDR numbering | PDR-002 source reuse, PDR-003 card launcher, PDR-004 module store, PDR-005 four modules | PDR-002 launcher + store, PDR-003 four modules, PDR-004 source reuse, PDR-005 transcript not primary, PDR-007 site report offline | Blueprint IDs kept. Mapping in [pdr/README.md](pdr/README.md). |
| BDR numbering | BDR-001 entitlements, BDR-002 subscription, BDR-003 metering, BDR-004 marketplace | BDR-001 business model, BDR-002 cost-aware pricing, BDR-003 entitlement ≠ permission, BDR-004 marketplace, BDR-005 enterprise revenue | Blueprint IDs kept. Mapping in [bdr/README.md](bdr/README.md). |
| Execution gates | G4 Platform Core … G13 Beta/Security/Observability | G4 Security Foundation … G13 Private Beta, G14 Paid Launch | Blueprint gates kept. Mapping in [execution-roadmap.md](../execution/execution-roadmap.md). |
| Event bus | "PostgreSQL Outbox + Dispatcher / NATS JetStream" | "Transactional Outbox + Event Bus abstraction" (no broker named) | Outbox + abstraction accepted (ADR-007); broker choice is [OD-007](open-decisions.md#od-007). |
| Entitlement tables | `entitlement_definitions`, `plan_entitlements`, `organization_entitlements` | `entitlements` (single table named in §7.3 and App. E) | Refinement: three tables realise the single Blueprint concept. See [entity-catalog.md](../data/entity-catalog.md). |
| Artifact lifecycle | Draft → Review → Changes Requested → Approved → Final | AI Draft → Review → Changes Requested → Approved → Final → **Exported** | Blueprint kept; "Exported" is a derived fact from `artifact_exports`, not a mutually exclusive state. See [approval-lifecycle.md](../artifacts/approval-lifecycle.md). |
| Migration order | 29 migrations, simplified table lists | App. E, includes `invitations`, `catalog_listings`, `upload_sessions`, `job_sources`, `outbox_events` | Blueprint order kept; one documented deviation (outbox/idempotency infrastructure created in migration `000`). See [migration-order.md](../data/migration-order.md). |

## Change Process

1. Author opens a pull request containing the new or changed decision record and all affected documents.
2. The owner of each affected canonical document reviews.
3. Technical Leadership (ADR), Product owner (PDR) or Business owner (BDR) accepts.
4. [traceability-matrix.md](traceability-matrix.md) and, if relevant, [open-decisions.md](open-decisions.md) are updated in the same pull request.

## Related Documents

- [documentation-conventions.md](documentation-conventions.md)
- [open-decisions.md](open-decisions.md)
- [traceability-matrix.md](traceability-matrix.md)
