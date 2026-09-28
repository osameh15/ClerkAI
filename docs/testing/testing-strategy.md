# Testing Strategy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead + QA |
| Blueprint | BP §19, §19.1, §29.2 |

## Layers (BP §19, extended)

| Layer | Tests | Tooling (indicative) | Doc | Blocks release |
|---|---|---|---|---|
| Unit | Domain rules, state machines, parsers, permissions, pricing calculations | Go `testing`, Vitest | [unit-testing](unit-testing.md) | Yes |
| Repository / Database | PostgreSQL queries + RLS, migrations, constraints | Testcontainers PostgreSQL | [database-testing](database-testing.md) | Yes |
| Contract | OpenAPI, event schemas, module manifests, JSON Schemas | OpenAPI validators, schema tests | [contract-testing](contract-testing.md) | Yes |
| API | Endpoint behaviour, errors, idempotency, pagination, authz matrix | Go HTTP tests against real server + DB | [api-testing](api-testing.md) | Yes |
| Integration | Object storage, Temporal, Redis, AI mocks/provider sandbox | Testcontainers, Temporal test server | [integration-testing](integration-testing.md) | Yes |
| Workflow | Temporal workflows: retries, cancel, resume, replay determinism | Temporal testsuite + replayer | [workflow-testing](workflow-testing.md) | Yes |
| Event | Outbox dispatch, consumer idempotency, ordering tolerance | Integration | [integration-testing](integration-testing.md) | Yes |
| Module | Manifest, workflow dry-run, mappings, templates, install/upgrade | module-spec CLI + integration | [module-testing](module-testing.md) | Yes |
| AI regression / Golden | Dataset-based evaluation | Evaluation runner | [ai-testing](ai-testing.md), [golden-tests](golden-tests.md) | Yes (AI changes) |
| Frontend | Components (all states, RTL/LTR), stores, SDK wrapper | Vitest + Vue Test Utils, visual regression | [frontend-testing](frontend-testing.md) | Yes |
| E2E | Login → Module → Upload → Job → Review → Export | Playwright | [e2e-testing](e2e-testing.md) | Yes |
| Security | Authorization matrix, BOLA, upload abuse, rate limits, injection, SSRF | Custom suites + DAST | [security-testing](security-testing.md) | Yes |
| Performance / Load | Upload concurrency, SSE, job queue, DB hot paths | k6-class tools | [performance-testing](performance-testing.md), [load-testing](load-testing.md) | Release candidates |
| Chaos / Resilience | Provider failure, worker crash, Temporal retry, storage timeout | Fault injection in staging | [chaos-testing](chaos-testing.md) | Gate-level |

Pyramid and ratios: [test-pyramid](test-pyramid.md). Release gates: [release-quality-gates](release-quality-gates.md).

## Principles

Real dependencies over mocks for persistence (PostgreSQL, Redis, MinIO, Temporal in containers); mocks only for external providers (AI, payment, email); deterministic AI mock provider for non-eval tests; every bug fix adds a regression test; tenant isolation tested at every layer.
