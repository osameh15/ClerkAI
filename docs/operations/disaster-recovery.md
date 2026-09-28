# Disaster Recovery

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Provisional (targets and topology) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |
| Blueprint | BP §14.4 (Paid Launch: DR test), §17.1 (RPO ≤ 15 min, RTO ≤ 4 h) |

## Targets

RPO ≤ 15 minutes for production metadata; RTO ≤ 4 hours at launch (BP §17.1).

## Strategy (launch)

Warm-standby-lite: IaC can recreate the full stack in a secondary region/zone; database restored from continuous backups; object storage replicated (or provider durability for single region initially, per [OD-005](../governance/open-decisions.md#od-005)); Temporal recreated and reconciled from PostgreSQL.

## Scenarios

| Scenario | Response |
|---|---|
| Zone failure | Managed HA failover (DB), redeploy stateless services |
| Region failure | Recreate in secondary region via IaC; restore DB; point DNS; announce degraded mode (uploads/processing may be paused until objects available) |
| Provider-wide AI outage | Not a DR event — degraded mode via fallback ([ai-provider-outage](runbooks/ai-provider-outage.md)) |
| Ransomware/credential compromise | Isolate, rotate credentials, restore from immutable backups, forensic preservation |

## DR Test

Annual full DR exercise (required before Paid Launch) and quarterly tabletop; results recorded with measured RPO/RTO.
