# Definition of Ready (Epic)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + Tech Lead |
| Blueprint | BP §29.1, §26.1 |

An epic is **Ready** when (BP §29.1):

- [ ] Business/user outcome is explicit.
- [ ] Domain owner and boundaries are identified (bounded context, tables, facades).
- [ ] API/schema/event changes are documented and reviewed (OpenAPI/JSON Schema/event catalogue PRs merged or approved).
- [ ] Permission and Entitlement impact is known (keys added to catalogues).
- [ ] Security/privacy considerations are written down (threat note; data classification; retention category).
- [ ] Telemetry and acceptance metrics are defined.
- [ ] Core test cases and failure modes are identified.

Additional (BP §26.1):

- [ ] Acceptance Criteria and test plan exist before implementation.
- [ ] Relevant open decisions (OD-xxx) for the epic are closed or explicitly deferred with a fallback.
- [ ] UX states (empty/loading/error/denied/upgrade/offline) designed where UI is involved.
