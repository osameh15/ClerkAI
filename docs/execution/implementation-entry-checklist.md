# Implementation Entry Checklist (Before the Four Real Modules)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Technical Leadership |
| Blueprint | App. F "Checklist Before Starting the Four Real Modules" |

| # | Item (BP App. F) | Evidence | Status |
|---|---|---|---|
| 1 | Hello Module appears in the Store from its Manifest without a Core code change | Sprint 1 S1-01 + grep check | ☐ |
| 2 | Install / Disable / Config and permission registration work end to end | Module lifecycle tests | ☐ |
| 3 | Source upload and reuse work; the original Source is immutable | Upload + reuse tests; immutability test | ☐ |
| 4 | Job failure / retry / cancel is tested through Temporal | Workflow tests | ☐ |
| 5 | Knowledge + Evidence + Review Item contracts are stable | Contract tests; ADR if changed | ☐ |
| 6 | Structured Artifact + versioning + PDF/DOCX prototype exists | S1-07/S1-08 + DOCX prototype | ☐ |
| 7 | AI Gateway has ≥ 2 provider/model adapters, or one provider plus a mock/fallback adapter | Adapter contract tests; [OD-016](../governance/open-decisions.md#od-016) | ☐ |
| 8 | Usage cost and audit are recorded through a complete vertical slice | Sprint 1 S1-10 | ☐ |
| 9 | Tenant-isolation integration tests run in CI | CI job | ☐ |
| 10 | Base Design System components exist for ModuleCard, SourceUploader, Processing, and ArtifactEditor | Component stories | ☐ |

Additional (documentation-derived):

| # | Item | Evidence | Status |
|---|---|---|---|
| 11 | Workflow DSL decision closed ([OD-018](../governance/open-decisions.md#od-018)) | ADR | ☐ |
| 12 | Golden dataset v1 for the first module started (≥ 30% cases) | Dataset record | ☐ |
| 13 | Evaluation runner produces results into `evaluation_*` tables ([OD-033](../governance/open-decisions.md#od-033)) | Run record | ☐ |

All items must be checked before EPIC-16..19 start.
