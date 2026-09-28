# Code Review Standards

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead |
| Blueprint | BP §26.1 (small, reviewable pull requests; ADR/migration/eval/threat notes) |

## Pull Request Requirements

- Small and focused (target < 400 changed lines excluding generated code).
- Description: intent, linked epic/story, risk, test evidence, screenshots for UI (all states), rollback plan for risky changes.
- Required attachments by change type: ADR (architecture), migration review (DB), evaluation report (AI), threat note (security-sensitive), OpenAPI diff (API).

## Reviewer Checklist

| Area | Questions |
|---|---|
| Boundaries | Does it respect context facades and table ownership? Any module-specific logic in Core? |
| Tenancy | Tenant context passed explicitly? RLS table? Cache/object keys tenant-prefixed? BOLA test added? |
| Authorization | Permission + entitlement + resource policy checks in the use case? |
| Idempotency | Commands idempotent? Activities/consumers safe to repeat? |
| Data | Immutable tables respected? Versioning? Retention category assigned? |
| Errors | Typed errors mapped to catalogue codes? No sensitive data in messages? |
| Observability | Spans/metrics/logs with correlation IDs; no content in logs |
| Security | Input validation, output sanitisation, SSRF, secrets |
| AI | Capability profile (not model) used? Output schema + citations? Evaluation run attached? |
| Tests | Right layer, deterministic, cover failure modes |
| Docs | Canonical docs updated |

Approvals: 1 reviewer for standard changes; 2 (incl. Tech Lead) for security-sensitive, tenancy, billing, or architecture changes.
