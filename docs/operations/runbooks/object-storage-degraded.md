# Runbook: Object Storage Degraded

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |

**Symptoms:** upload session creation 503, uploads failing, processing activities retrying on IO, exports failing, evidence viewer media not loading.

**Detection:** storage error-rate alert; upload completion failures.

**Immediate actions:** declare incident; confirm provider status; PWA offline queues retain captures (communicate "uploads paused, captures safe").

**Diagnosis:** provider outage vs credentials/IAM change vs bucket policy change vs rate limiting.

**Mitigation:** revert IAM/policy changes; reduce concurrency (provider throttling); pause ingestion queues to avoid retry storms (Temporal retries continue with backoff).

**Recovery:** resume queues; clients resume multipart uploads; verify checksum verifications pass.

**Validation:** upload success > 99%; processing backlog cleared; sample evidence media loads.

**Escalation:** provider support; Tech Lead.

**Post-incident:** consider multi-region replication if repeated.
