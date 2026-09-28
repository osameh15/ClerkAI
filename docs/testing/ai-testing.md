# AI Testing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer |
| Blueprint | BP §19 (AI Eval: Dataset-based regression), §11.4 |

## Two Tracks

| Track | Purpose | Provider | Determinism |
|---|---|---|---|
| Functional AI tests | Gateway logic, routing, fallback, validation, mapping into Knowledge | Mock provider with scripted responses | Deterministic |
| Quality evaluation | Model/prompt quality | Real providers (budget-capped keys) | Statistical; thresholds + tolerances |

## Mock Provider

A local service implementing the provider contract: returns fixture outputs keyed by (task, input hash), can simulate latency, 429/5xx, malformed JSON, schema violations, truncation, content rejection. Required for local/CI and Sprint 1 ([provider-contract](../ai/provider-contract.md)).

## Security Tests for AI

Prompt injection corpus (fa/en, hidden PDF text, spoken instructions, slide notes); assertions: schema compliance, no unsupported claims, no cross-job data, no policy/behaviour change ([prompt-injection](../security/prompt-injection.md)).

## Evaluation Runs

See [evaluation-strategy](../ai/evaluation/evaluation-strategy.md), [golden-tests](golden-tests.md), [regression-gates](../ai/evaluation/regression-gates.md).
