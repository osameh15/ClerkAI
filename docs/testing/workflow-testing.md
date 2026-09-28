# Workflow Testing (Temporal)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Processing domain |
| Blueprint | BP §19 (Integration: Temporal), G7 ("Workflow failure/recovery tests pass"), App. D (Job reliability), App. F (Temporal hello workflow + retry/cancellation test) |

| Test | Method |
|---|---|
| Unit workflow tests | Temporal test environment with mocked activities: DAG interpretation, branching, review waits (signals), timers (escalation), cancellation, compensation |
| Activity idempotency | Execute each activity twice with same inputs → no duplicate side effects |
| Retry policies | Inject transient errors → retries with backoff; non-retryable → fail fast |
| Worker crash | Integration: kill worker mid-activity (heartbeating) → resumes on another worker; job completes; no duplicate AIExecutions except recorded retries |
| Temporal outage | Stop Temporal during submit → job `queued` in DB → reconciler starts workflow after recovery |
| Replay determinism | Record histories from integration runs; CI replays them against new worker code (`Replayer`); any non-determinism fails the build |
| Versioning | Tests for `GetVersion` branches with old and new histories |
| Human waits | Review/approval signals after long timer skips (time-skipping test server) |
