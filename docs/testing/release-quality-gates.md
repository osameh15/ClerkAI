# Release Quality Gates

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead + QA |
| Blueprint | BP §16.1, §18.1, §19, §29.2 |

## Failures That Block a Release

| Category | Blocking condition |
|---|---|
| Build | Lint/format/static analysis/architecture-boundary failures |
| Tests | Any failing unit, integration, contract, API, workflow, module, frontend or E2E test |
| Tenant isolation | Any RLS/BOLA failure (no exceptions) |
| API | OpenAPI invalid or breaking change on `/v1` without major version |
| Migrations | Migration test failure; unannotated destructive change |
| Security | New Critical/High vulnerabilities without approved exception; secret detected |
| AI | Evaluation regression gate failure (quality **or** cost) for affected modules |
| Workflow | Replay determinism failure |
| Exports | Golden PDF/DOCX failures |
| Operability | Missing required telemetry/alerts for new features (DoD), missing rollback path for high-risk change |

## Non-Blocking (require acknowledgement)

Coverage drop, non-blocking evaluation metrics regression, performance regressions within tolerance, flaky test quarantined with ticket (max 7 days).

Gate-level (roadmap) criteria: [release-gates](../execution/release-gates.md). Security release gates: [security-release-gates](../security/security-release-gates.md).
