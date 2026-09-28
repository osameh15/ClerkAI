# Non-Goals

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §1.2 |

## Product Non-Goals (BP §1.2)

| Non-goal | Example feature requests to reject | Acceptable adjacent capability |
|---|---|---|
| General-purpose ERP | Inventory, procurement, accounting | Knowledge entity type `amount` for figures mentioned in sources |
| General-purpose CRM | Deals, pipelines, contact enrichment | `person`/`organization` entities with evidence |
| Full PM suite (Jira/ClickUp) | Boards, sprints, dependencies, Gantt | Action Items with owner, deadline, status, and later webhooks to PM tools |
| Only voice-to-text or OCR | "Just give me the transcript" as a product | Transcript as secondary output ([PDR-005](../governance/pdr/PDR-005-transcript-not-primary.md)) |
| Chat interface on an LLM | Open chat as the main UX, model picker | Scoped Q&A over a Job's Knowledge (future, grounded, evidence-backed) |
| Arbitrary third-party code in main process (v1) | Plugin `.so`, uploaded JS/Go code | Declarative modules; future remote modules via gateway |

## Engineering Non-Goals for v1

- No microservices split ([ADR-002](../governance/adr/ADR-002-modular-monolith.md)).
- No Kubernetes unless scale justifies it ([OD-029](../governance/open-decisions.md#od-029)).
- No proprietary foundation-model training ([training-strategy](../ai/training/training-strategy.md)).
- No WebSocket until live collaboration is scoped ([ADR-021](../governance/adr/ADR-021-sse-progress.md)).
- No premature partitioning or vector index tuning (BP App. E).
