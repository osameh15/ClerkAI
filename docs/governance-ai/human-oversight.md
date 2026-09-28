# Human Oversight

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §1.1 (Human-in-the-loop), §14.3 ("Low-confidence or high-risk outputs require human review"), §16.2 |

## Oversight Layers

| Layer | Mechanism | Configurable by |
|---|---|---|
| Item-level review | Review items for low confidence, missing evidence, conflicts, ambiguity | Module thresholds; org can raise thresholds |
| Document-level approval | Approval workflows | Org (entitlement) |
| Draft labelling | Non-final artifacts labelled Draft | Always on |
| Finalisation gate | Blocking review items prevent finalisation | Module (`blocksFinalization`) — org cannot disable for evidence-required high-risk types |
| High-risk types | Always reviewed regardless of confidence (for example Site Report `critical` severity, handwriting numbers/dates below threshold) | Module policy |
| Oversight of automation | No auto-sending of documents or actions to external parties in v1 | Platform rule |

## Human Review Defaults

Organizations set defaults on the AI Policy screen: require review before artifact generation (per module), require approval before export of Final documents, reviewer assignment rules.
