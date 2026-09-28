# Release Gates

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Technical Leadership |
| Blueprint | BP §14.4, §16.1, §24 |

## Levels

| Level | Scope | Criteria |
|---|---|---|
| Per release | Every production deploy | [release-quality-gates](../testing/release-quality-gates.md) |
| AI changes | Model/prompt/schema/profile | [quality-gates](../governance-ai/quality-gates.md) |
| Security stage gates | Alpha / Private Beta / Paid Launch / Enterprise | [security-release-gates](../security/security-release-gates.md) |
| Roadmap gates | G0–G14 | [execution-roadmap](execution-roadmap.md) |

## Stage Release Checklist (Private Beta example)

- G11 module acceptance criteria demonstrated in staging.
- Private Beta security gate evidence (dependency/container scans, file scanning, incident runbook, backup restore test).
- SLO dashboards and alerts live; on-call rota defined.
- Support process and feedback channel ready; in-product feedback enabled.
- Data processing terms and privacy notices published for pilot organizations.
- Pricing sandbox (G12) working; pilot plans configured.
