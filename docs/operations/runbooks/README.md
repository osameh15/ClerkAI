# Runbooks

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |
| Blueprint | BP §14.4 (incident runbook), §17 |

Every runbook follows: **Symptoms · Detection · Immediate actions · Diagnosis · Mitigation · Recovery · Validation · Escalation · Post-incident follow-up**. Runbooks are exercised in game days before Private Beta.

| Runbook | Trigger |
|---|---|
| [api-degraded](api-degraded.md) | API errors/latency, outbox lag |
| [database-degraded](database-degraded.md) | PostgreSQL issues |
| [redis-degraded](redis-degraded.md) | Redis unavailable/slow |
| [temporal-degraded](temporal-degraded.md) | Temporal unavailable/backlog |
| [object-storage-degraded](object-storage-degraded.md) | Storage errors |
| [ai-provider-outage](ai-provider-outage.md) | Provider/route failures |
| [ai-cost-spike](ai-cost-spike.md) | Cost anomaly |
| [stuck-workflow](stuck-workflow.md) | Jobs not progressing |
| [failed-upload](failed-upload.md) | Upload/scan/processing failures |
| [webhook-delivery-failures](webhook-delivery-failures.md) | Outbound webhook failures |
| [tenant-security-incident](tenant-security-incident.md) | Cross-tenant exposure, malware campaign, audit tamper |

Severity and process: [incident-response](../incident-response.md).
