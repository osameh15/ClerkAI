# Technical Debt Policy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead |
| Blueprint | BP §27 (over-engineering vs rewrite risk), §1.1 (Scale by extraction) |

## Rules

1. Debt is recorded (issue with `tech-debt` label: description, impact, remedy, trigger/deadline) — never only in comments.
2. **Non-negotiable (no debt allowed):** tenant isolation, authorization, audit writes, idempotency of billing-relevant operations, immutability of originals/artifact versions, secret handling.
3. **Acceptable deliberate shortcuts:** performance optimisations deferred until measured, admin UIs as CLIs, limited offline sophistication, single-provider + mock adapters before G8, simple templates.
4. 15–20% of sprint capacity reserved for debt and reliability after G11.
5. Stale feature flags older than 60 days and unused indexes are reviewed monthly.
6. Architecture boundary violations are treated as bugs, not debt.
7. Every deferred item that affects a Blueprint requirement is mirrored in the [traceability-matrix](../governance/traceability-matrix.md) status.
