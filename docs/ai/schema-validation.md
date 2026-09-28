# Schema Validation

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI domain |
| Blueprint | BP §11, §14.3 ("Structured model output is validated and sanitized before use") |

## Validation Layers

| Layer | Checks |
|---|---|
| 1. Syntax | Strict JSON parse (no trailing text; code fences stripped only if exact) |
| 2. Schema | JSON Schema 2020-12 validation (compiled, cached per schema ref); `additionalProperties: false` in AI output schemas |
| 3. Semantics | Citation handles exist; quotes match; enum values valid; dates parseable; numeric ranges; referential consistency (relations reference extracted items) |
| 4. Sanitisation | Unicode normalisation; strip control/bidi-override characters that could spoof text; no HTML/markup; URLs only https and only if the task allows links; length caps |

## Implementation Notes

- Schemas are loaded from module packages and platform schema store by `schema_ref`; the validator library supports 2020-12 (for example `santhosh-tekuri/jsonschema` in Go — library choice is an implementation detail).
- Validation errors are summarised (paths + messages, no content) for repair prompts and for AIExecution error details.
- Target metric: schema validity after retry/fallback ≥ 99.5% on benchmark (BP App. D).
