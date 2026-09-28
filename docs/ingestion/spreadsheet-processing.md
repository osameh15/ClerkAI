# Spreadsheet Processing (XLSX, CSV)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Sources domain |
| Blueprint | BP §0.1 (XLSX), §9 |

## Behaviour

- Parse sheets, used ranges, header detection, merged cells; cell **values** (not formulas' execution) — cached values used; formulas stored as text metadata only.
- Output `tables` extraction: one table per detected region with `sheet` + `cell_range` locators ([evidence.schema.json](../api/schemas/evidence.schema.json) `page` pointer with `sheet`, `cell_range`).
- Large sheets: cap rows/cells per sheet (for example 50k cells) for AI consumption; summary statistics produced instead of full dumps.
- External links and data connections ignored; macros rejected.

## Use in v1 Modules

Supporting documents in Meeting Minutes (for example budget sheet); not a primary input for launch modules.
