# ADR-010: AI Output = Structured + Schema-Validated

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership, AI Engineer |
| Blueprint | BP §1.1, §11, §14.3, §28.1, App. D |

## Context

Free-text model output cannot be validated, traced or reliably rendered into business documents, and it is a vector for injection into renderers.

## Decision

- Every AI call that produces business data declares an **output JSON Schema** (versioned, referenced by the PromptVersion).
- The AI Gateway validates output against the schema. Invalid output → repair attempt (bounded) → fallback model → failure. Each attempt is its own AIExecution.
- Valid output is mapped into **Knowledge** (entities/relations/evidence) — not directly into final Artifacts.
- Artifact generation consumes Knowledge; rendering never executes model-provided HTML/URLs without sanitisation.
- Target: ≥ 99.5% of structured outputs schema-valid after retry/fallback on benchmark (BP App. D).

## Consequences

Prompts, schemas and mapping code are versioned together; schema changes run evaluation before rollout.

## Related

[structured-output](../../ai/structured-output.md), [schema-validation](../../ai/schema-validation.md), [output-validation](../../security/output-validation.md)
