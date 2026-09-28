# Structured Output

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI domain |
| Blueprint | BP §11, §14.3, [ADR-010](../governance/adr/ADR-010-structured-ai-output.md), App. D (≥ 99.5% schema-valid after retry/fallback) |

## Rules

1. Every business-data AI task has an output JSON Schema.
2. Use provider-native structured output/JSON mode when available; otherwise prompt-constrained JSON with strict parsing.
3. Output schemas include, for each extracted item: value fields, `citations` (chunk handles, optionally with offsets/timestamps), optional `confidence`, and `uncertainty_reason`.
4. Output is mapped to Knowledge by platform mapping code declared per task (module `outputSchema` + mapping config), not by ad-hoc code.

## Handling Matrix (brief §32)

| Condition | Detection | Handling |
|---|---|---|
| Malformed JSON | Parse error | Repair attempt (same route) → fallback route → fail step |
| Schema violation | JSON Schema validation | Repair with validator messages → fallback → fail; partial acceptance is **not** allowed for required structures |
| Missing fields | Required property missing | Same as schema violation; optional missing → item created without field + Review Item if field is evidence-required |
| Conflicting sources | Fusion detects contradictions | `CONTRADICTS` relation + `conflicting_sources` Review Item |
| Low-confidence values | Confidence/threshold | Review Item; band shown |
| Hallucinated data | Citation to non-provided chunk; quote not found in chunk; entity with no citation where required | Drop citation/item; Review Item `missing_evidence`; hallucination metric counter ([hallucination-controls](hallucination-controls.md)) |
| Unsupported language | Language detection vs profile languages | Route to profile supporting it; else fail `AI_LANGUAGE_UNSUPPORTED` with user-visible explanation |
| Provider failure | Adapter error class | Retry/fallback ([fallback-strategy](fallback-strategy.md)) |
| Truncated output (length) | `finish_reason=length` | Split task (fewer chunks per call) and retry |
