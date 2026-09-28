# Module Catalog

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §3, §5.3, §23 |

## Module Key Convention

`<category>.<name>` — lowercase, kebab-case name, globally unique, immutable once published. Category is a catalog taxonomy value, not a code path. See [manifest-reference](../modules/sdk/manifest-reference.md).

## v1 Official Modules

| Key | Category | Publisher | Stage | Primary entitlement | Spec |
|---|---|---|---|---|---|
| `business.meeting-minutes` | business | clerk-official | v1 | `module.business.meeting-minutes` | [spec](../modules/meeting-minutes/README.md) |
| `construction.site-report` | construction | clerk-official | v1 | `module.construction.site-report` (Construction Pack add-on) | [spec](../modules/site-report/README.md) |
| `general.handwriting-document` | general | clerk-official | v1 | `module.general.handwriting-document` | [spec](../modules/handwriting-document/README.md) |
| `education.lecture-summary` | education | clerk-official | v1 | `module.education.lecture-summary` | [spec](../modules/lecture-summary/README.md) |
| `platform.hello` | platform | clerk-official | Sprint 0/1 only (internal) | `module.platform.hello` | [sprint-1](../execution/sprint-1.md) |

Note: BP App. C lists the Construction Pack as an add-on on every plan while "Four Launch Modules" are "Limited" on Personal. Exact packaging of Site Report inside the Construction Pack vs base plans is a pricing decision ([OD-021](../governance/open-decisions.md#od-021)); the entitlement key design supports either.

## Future Candidates (not committed)

| Candidate | Category | Source | Notes |
|---|---|---|---|
| Daily Progress Report | construction | BP §5.3 (after v1) | Construction Pack |
| Inspection Report | construction | BP §5.3 (after v1) | Construction Pack |
| HSE Report | construction | BP §5.3 (expansion) | |
| NCR | construction | BP §5.3 (expansion) | |
| RFI | construction | BP §5.3 (expansion) | |
| Change Order | construction | BP §5.3 (expansion) | |
| Interview Summary, Candidate Evaluation | hr | brief | |
| Business Letter, Proposal Generator, Contract Summary, Consulting Report | business | brief | |
| Technician Service Report | field-service | brief | |
| Board Minutes | business | brief | Likely a `meeting-minutes` dependency/variant |
| Study Notes, Exam Builder | education | brief | Reuse lecture Knowledge |

**Platform test:** each candidate must be implementable with a new manifest, schemas, prompts, templates, and at most official native steps — with no changes to Authentication, Organizations, Permissions, Entitlements, Uploads, AI Gateway, Knowledge, Evidence, Artifact Engine, Workflow, Billing, Audit or Module Store.
