# Hallucination Controls

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI domain |
| Blueprint | BP §11.4 (Hallucination: unsupported-claims rate), §14.3, App. D ("critical unsupported action < 1%", "No silent invention", "Out-of-source educational claims are minimized and flaggable in source-grounded mode") |

## Controls

| # | Control | Mechanism |
|---|---|---|
| H1 | Grounded prompting | Only provided chunks are allowed as sources; instructions to return `null` when unsupported |
| H2 | Mandatory citations | Output schema requires citations for evidence-required fields |
| H3 | Citation verification | Handles must exist in the prompt; quotes must match chunk text (normalised); timestamps/pages within chunk locator |
| H4 | Unsupported-claim filter | Items without valid citation are dropped or become Review Items (per type policy); never silently kept for evidence-required types |
| H5 | Numeric/date verification | Numbers and dates in outputs must appear (normalised, incl. Persian digits and Jalali forms) in cited chunks or be derivable by declared rules (relative dates) |
| H6 | Source-grounded mode | Lecture module default: generated study material must cite; content without citation is flagged "outside source" and hidden or marked per configuration |
| H7 | No free-text artifacts | Artifacts generated from Knowledge, not directly from model prose ([ADR-010](../governance/adr/ADR-010-structured-ai-output.md)) |
| H8 | Human review | Low confidence/high-risk → review; final requires resolution of blocking items |
| H9 | Measurement | Unsupported-claims rate per module in evaluation and sampled production audits ([evaluation-metrics](evaluation/evaluation-metrics.md)) |

## Metrics

`hallucination.unsupported_claim_rate`, `hallucination.critical_unsupported_action_rate` (Meeting), `hallucination.silent_invention_rate` (Handwriting, target 0), `hallucination.out_of_source_claim_rate` (Lecture).
