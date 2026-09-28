# Testing Modules

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform + QA |
| Blueprint | BP §19 (Contract: Module manifests), §19.1 |

## Test Layers for a Module

| Layer | What | Tooling | Blocking |
|---|---|---|---|
| Manifest contract | `module-spec validate` L1–L7 | CLI in CI | Yes |
| Schema fixtures | Valid/invalid examples for each schema | JSON Schema tests | Yes |
| Workflow definition | DAG validity, step params, dry-run with mock provider producing fixture outputs | Temporal test env + mock gateway | Yes |
| Mapping tests | Knowledge fixture → artifact content (deterministic mappings) | Unit | Yes |
| Template golden tests | Artifact fixtures → PDF/DOCX golden outputs | Renderer golden tests | Yes |
| Install/upgrade | Install on test org; upgrade from previous version with config transform fixtures | Integration | Yes |
| E2E happy path | Create job → upload fixture → process (mock AI) → review → artifact → export | E2E | Yes (per module) |
| Evaluation | Golden suites with real providers | Evaluation runner | Release gate |

## Mock Provider Fixtures

Each AI task has recorded fixture outputs keyed by input hash, so module E2E tests are deterministic and free of provider cost.
