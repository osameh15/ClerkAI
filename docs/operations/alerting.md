# Alerting

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (thresholds Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |
| Blueprint | BP §17, §22.1 (anomaly detection) |

## Principles

Alert on symptoms (SLO burn) first, causes second; every page links to a runbook; no alert without an owner.

## Alerts (initial)

| Alert | Condition | Severity | Runbook |
|---|---|---|---|
| API availability burn | Multi-window burn rate > 14.4 (1 h) / 6 (6 h) | Page | [api-degraded](runbooks/api-degraded.md) |
| API latency | p95 > 1 s for 10 min | Page | [api-degraded](runbooks/api-degraded.md) |
| DB replication lag / connections saturated | lag > 30 s; connections > 90% | Page | [database-degraded](runbooks/database-degraded.md) |
| Redis unavailable | health fail 2 min | Ticket (page if > 15 min) | [redis-degraded](runbooks/redis-degraded.md) |
| Temporal task latency | schedule-to-start > 5 min on any queue | Page | [temporal-degraded](runbooks/temporal-degraded.md) |
| Stuck workflows | runs in `running` without step progress > 2 h (excluding review waits) | Ticket | [stuck-workflow](runbooks/stuck-workflow.md) |
| Object storage errors | error rate > 5% for 5 min | Page | [object-storage-degraded](runbooks/object-storage-degraded.md) |
| Upload completion failures | > 5% in 30 min | Ticket | [failed-upload](runbooks/failed-upload.md) |
| AI provider degraded | route error rate > 20% for 5 min / circuit open | Ticket (page if all routes of a profile down) | [ai-provider-outage](runbooks/ai-provider-outage.md) |
| AI cost spike | > 3× baseline hourly cost (org or global) | Page (global) / ticket (org) | [ai-cost-spike](runbooks/ai-cost-spike.md) |
| Outbox lag | > 60 s for 5 min | Ticket | [api-degraded](runbooks/api-degraded.md) |
| Webhook failures | endpoint failing rate spike | Ticket | [webhook-delivery-failures](runbooks/webhook-delivery-failures.md) |
| Security: RLS guard trip / cross-tenant anomaly | any | Page (SEV1) | [tenant-security-incident](runbooks/tenant-security-incident.md) |
| Malware detections spike | > N/hour | Ticket | [tenant-security-incident](runbooks/tenant-security-incident.md) |
| Backup failure | any failed backup | Ticket | [backup](backup.md) |
| Audit hash chain break | any | Page | [tenant-security-incident](runbooks/tenant-security-incident.md) |
