# Definition of Done

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead + QA |
| Blueprint | BP §29.2, §26.1 |

A change/story/epic is **Done** when (BP §29.2):

- [ ] Code review is complete and tests pass.
- [ ] OpenAPI/schema/docs are updated.
- [ ] Migration is tested and backward compatibility reviewed.
- [ ] Security scans contain no blockers.
- [ ] Required logs, metrics, traces and alerts are implemented.
- [ ] Audit/Usage events are recorded where applicable.
- [ ] Feature flag/rollback exists for high-risk changes.
- [ ] User-facing UX includes empty, loading, error and access-denied states (plus entitlement-upgrade and offline where relevant).
- [ ] AI features pass Evaluation regression.
- [ ] End-to-end Acceptance Criteria are demonstrated in Staging.

Clerk-specific additions:

- [ ] Tenant-isolation tests cover new tenant tables/endpoints.
- [ ] Architecture boundary lint passes (no cross-context internals, no module literals in Core).
- [ ] Traceability matrix / relevant docs updated if behaviour or contracts changed.
- [ ] AI changes have an evaluation report; DB changes have migration review; security-sensitive changes include a threat note (BP §26.1).
