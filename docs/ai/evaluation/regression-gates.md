# Regression Gates

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer + Tech Lead |
| Blueprint | BP §11.4, §16.1 ("Quality regression and cost regression can both block a release"), §18.1 (AI eval smoke), §29.2 ("AI features pass Evaluation regression") |

## Gate Rules

A change is **blocked** if any of:

1. Any **blocking** metric falls below its absolute threshold (module suite `min`/`max`).
2. Any blocking metric regresses vs the current active baseline by more than its tolerance (default: F1/accuracy −2 pp; hallucination rates +0.5 pp; schema validity −0.3 pp).
3. Cost per case increases > 20% without an approved cost exception.
4. p95 latency increases > 30% without approval.
5. Any **critical** case (tagged `critical`) fails (for example a known past incident).

**Warn** (non-blocking, requires reviewer acknowledgement): non-blocking metrics regress; calibration ECE worsens.

## Scope of Gate by Change Type

| Change | Suites |
|---|---|
| Prompt version | Suites of tasks using that prompt |
| Output schema | Same + artifact generation tests |
| Profile route/model | All suites using the profile (cross-module) |
| Ingestion pipeline version | Transcription/OCR suites + downstream module suites |
| Module version with AI config changes | Module suites |
| Platform knowledge/fusion code | All module suites (nightly + RC) |

## Evidence of Pass

Gate result stored (`evaluation_runs.gate_result`) and linked in the PR/release; model/prompt promotion APIs refuse without a passing run ID.
