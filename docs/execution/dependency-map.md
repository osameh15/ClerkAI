# Dependency Map

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead |
| Blueprint | BP §24, §25 |

## Epic Dependencies

```mermaid
flowchart TB
  E01[EPIC-01 Identity & Org] --> E02[EPIC-02 Access & Policy]
  E01 --> E03[EPIC-03 Commerce Core]
  E02 --> E04[EPIC-04 Module Registry]
  E03 --> E05[EPIC-05 Module Installation]
  E04 --> E05
  E02 --> E06[EPIC-06 Source Platform]
  E06 --> E07[EPIC-07 Source Processors]
  E05 --> E08[EPIC-08 Jobs & Temporal]
  E06 --> E08
  E08 --> E09[EPIC-09 AI Gateway]
  E07 --> E09
  E09 --> E10[EPIC-10 Prompt / Eval]
  E09 --> E11[EPIC-11 Knowledge]
  E11 --> E12[EPIC-12 Artifact Engine]
  E12 --> E13[EPIC-13 Templates]
  E12 --> E14[EPIC-14 Workflow / Approval]
  E05 --> E15[EPIC-15 Home & Store UX]
  E13 & E14 & E10 & E15 --> E16[EPIC-16 Meeting]
  E13 & E14 & E10 & E15 --> E17[EPIC-17 Site Report]
  E13 & E14 & E10 & E15 --> E18[EPIC-18 Handwriting]
  E13 & E14 & E10 & E15 --> E19[EPIC-19 Lecture]
  E01 --> E20[EPIC-20 Audit / Privacy]
  E01 --> E21[EPIC-21 Observability / Ops]
  E03 --> E22[EPIC-22 Billing & Launch]
```

EPIC-20 (audit) and EPIC-21 (observability) start in Sprint 0 as foundations and grow with every epic. EPIC-17 additionally depends on PWA offline capabilities.

## External/Decision Dependencies

| Dependency | Needed by | OD |
|---|---|---|
| Router, SQL library, migration tool, monorepo tooling | Sprint 0 | Closed: ADR-023, ADR-024, ADR-025, ADR-026 (+ ADR-027 Go 1.27.x) |
| Identity provider | G4 | OD-004 |
| Secret manager | G4 | OD-013 |
| Object storage (prod), AV engine | G6 | OD-006, OD-011 |
| Workflow DSL | G5/G7 | OD-018 |
| ASR/LLM providers | G8 | OD-015, OD-016 |
| Renderers, UI kit/editor, template format | G10 | OD-008, OD-009, OD-014, OD-038 |
| Payment provider, pricing | G12 | OD-010, OD-021 |
| Hosting, runtime platform | G13 | OD-005, OD-029 |
