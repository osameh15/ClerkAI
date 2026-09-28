# Evaluation Reference

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §4 (evaluations/), §11.4 |

## Declaring Suites

In the manifest `evaluation.suites[]`: `key`, `dataset` (`key@range`), `tasks` (manifest AI task keys), `metrics[] {key, min|max, blocking}`, `releaseGate`.

## Package Contents (`evaluations/`)

| File | Purpose |
|---|---|
| `suites/<suite>.yaml` | Scorer configuration per metric (matchers, tolerances, judge prompt versions) |
| `annotation-guidelines.md` | Definitions used by annotators (for example Decision vs Proposal) |
| `fixtures/` | Small deterministic cases for unit/contract tests with the mock provider |

## Metric Keys

Use keys from [evaluation-metrics](../../ai/evaluation/evaluation-metrics.md); new metric keys require a scorer implementation in the platform evaluation runner (reviewed by AI Engineer).

## Running

`module-spec eval --suite meeting-core --subject prompt=meeting.action-extraction@1.1.0 --profile high_quality_extraction` (CLI via runner, [OD-033](../../governance/open-decisions.md#od-033)); CI runs smoke subsets on PRs.
