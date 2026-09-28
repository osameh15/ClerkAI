# DOCX Export

| Field | Value |
|---|---|
| Status | Proposed |
| Spec class | Provisional (engine Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Artifacts domain |
| Blueprint | BP §0.1, §12, §27 (DOCX fidelity risk) |

## Requirements

| Requirement | Detail |
|---|---|
| Editability | Real Word structures: headings with styles, lists, tables (not images of text) |
| Styles | Template styles mapped from TemplateVersion + Brand Kit (fonts, colours) |
| RTL | `w:bidi` paragraphs, `w:rtl` runs, correct table direction |
| Header/footer | Logo, document number, page fields |
| Images | Embedded renditions |
| Metadata | Core properties (title, company) |
| Determinism | Stable XML for golden tests |

## Engine Decision

[OD-009](../governance/open-decisions.md#od-009). Entitlement: `export.docx` (BP App. C does not list DOCX separately; plan mapping per [plan-entitlements](../platform/entitlements/plan-entitlements.md) — provisional: all paid plans).

## TBD

Engine and whether DOCX is generated from the same template definition as PDF or from a DOCX reference template ([OD-038](../governance/open-decisions.md#od-038)).
