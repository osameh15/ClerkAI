# PDR-006: Human Review and Evidence Are Core Product Value

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Product |
| Blueprint | BP §1.1, §10, §16.2, §28.2 |

## Decision

- Review of low-confidence items and one-click access to Evidence are part of every module's core flow, not optional extras.
- Review states: `ai_generated`, `human_confirmed`, `human_modified`, `rejected`, `unresolved` (BP §10.2).
- Output is labelled Draft until confirmation/approval; AI-generated and human-edited sections are distinguishable where relevant.

## Consequences

Review Queue, Evidence Viewer and ConfidenceBadge are platform components. Acceptance criteria require that ambiguous values create Review Items rather than guesses.

## Related

[review](../../platform/collaboration/review.md), [human-correction](../../knowledge/human-correction.md), [evidence-ui](../../frontend/evidence-ui.md)
