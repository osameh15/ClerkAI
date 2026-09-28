# Resumability

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Processing domain |
| Blueprint | App. D ("Worker crash/timeout never loses a Job; Temporal resume is demonstrated"), App. F |

## Levels

| Level | Mechanism |
|---|---|
| Worker crash / deploy | Temporal replays workflow history; in-flight activity retried on another worker (heartbeat/start-to-close timeouts) |
| Long activities | Heartbeat with progress details (for example processed audio segment index); on retry, resume from heartbeat details |
| Segmented work | Long audio/PDF split into idempotent sub-activities keyed by segment; completed segments skipped |
| Manual retry after failure | New run reuses successful step outputs when inputs and pinned versions are unchanged (step output cache keyed by `(job_id, step_key, input_hash, module_version)`); AI calls not repeated |
| Temporal persistence loss (disaster) | Reconciler recreates workflows from PostgreSQL run/step state (PostgreSQL is the record) |

## Test Requirement

Sprint 1/G7 must demonstrate: kill a worker mid-transcription and mid-extraction; job completes without duplicate AIExecutions beyond retry attempts and without duplicate artifacts ([workflow-testing](../testing/workflow-testing.md)).
