# Numbering Sequences

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Templates domain |
| Blueprint | BP §3.1 ("organization's template and document numbering"), §12 ("Numbering: Transaction-safe sequence and template-based numbering") |

## Model

`numbering_sequences(organization_id, key, pattern, scope_key, next_value, reset_policy)`.

- `key` from the manifest (`meeting-minutes`, `site-report`, `outgoing-letter`) or organization-defined.
- `pattern` tokens: `{yyyy}`/`{yy}` (Gregorian), `{jyyyy}`/`{jyy}` (Jalali), `{mm}`, `{project}` (project code), `{module}`, `{seq:N}` (zero-padded).
- `scope_key`: computed from pattern scope (for example per project per year) → separate counters.
- `reset_policy`: `never`, `yearly` (Gregorian or Jalali year per organization calendar setting).

## Allocation

- Allocated at finalisation (default) or at approval submission (`assignOn`), inside the same transaction as the status change: `UPDATE … SET next_value = next_value + 1 RETURNING next_value - 1` (row lock) → gap-free per scope under normal operation.
- A number, once allocated, is never reused, even if the transaction later fails after commit (it cannot) or the artifact is deleted (gap is acceptable and audited).
- Idempotent: number stored on the artifact; re-running finalisation returns the same number.

## Admin

Organizations with `templates.manage` configure patterns and starting values; changes audited; changing a pattern never renumbers existing documents.
