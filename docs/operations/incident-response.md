# Incident Response

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE + Security |
| Blueprint | BP §14.4 (Private Beta: incident runbook), §16, §17 |

## Severity

| Sev | Definition | Response |
|---|---|---|
| SEV1 | Security breach/cross-tenant exposure; full outage; data loss | Immediate page; incident commander; exec + legal notified; customer comms within contractual/legal timelines |
| SEV2 | Major feature down (processing, uploads), AI quality incident affecting final documents, cost incident | Page; IC assigned; status page |
| SEV3 | Degraded performance, partial feature issue | Business hours |
| SEV4 | Minor, no customer impact | Ticket |

## Process

1. **Detect** (alert, customer report, security finding).
2. **Declare** (severity, IC, channel, timeline doc).
3. **Mitigate** (runbooks; flags; rollback; isolate).
4. **Communicate** (status page, affected org admins; for security/privacy incidents follow notification obligations per jurisdiction/contract).
5. **Resolve** and **verify**.
6. **Postmortem** (blameless) within 5 business days for SEV1/2: timeline, root cause, contributing factors, action items with owners; AI incidents add regression cases ([incident-management](../governance-ai/incident-management.md)).

## Evidence Preservation

Security incidents: preserve logs, audit exports, snapshots before remediation that destroys evidence.

Runbooks: [runbooks/README](runbooks/README.md).
