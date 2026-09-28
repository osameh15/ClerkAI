# AI Incident Management

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer + SRE |
| Blueprint | BP §16 ("failure response"), §17 |

## AI Incident Types

| Type | Examples | Severity guide |
|---|---|---|
| Quality regression | Correction rate spike after rollout; systematic wrong deadlines | SEV2 (SEV1 if final documents affected at scale) |
| Harmful/unsafe output | Offensive content in artifact, unsafe severity downgrade | SEV2 |
| Data exposure via AI | Cross-tenant content in prompt/output, system prompt leakage, provider data incident | SEV1 (also security incident) |
| Provider outage | All routes of a profile down | SEV2/SEV3 by duration |
| Cost incident | Runaway loop, abuse | SEV2 |
| Prompt injection success | Document content changed behaviour beyond schema | SEV2 (SEV1 if exfiltration) |

## Response Steps

1. **Contain:** disable flag/route, roll back prompt version, pause affected module processing (org or global kill switch).
2. **Assess:** identify affected jobs/artifacts via provenance (prompt version, model, time window) — provenance queries are pre-built.
3. **Remediate:** re-run affected jobs (new runs) where safe; mark affected artifact versions with a warning; notify affected organizations when final documents may be impacted.
4. **Learn:** add failing cases to regression datasets (tag `critical`); postmortem ([incident-response](../operations/incident-response.md)).

Runbooks: [ai-provider-outage](../operations/runbooks/ai-provider-outage.md), [ai-cost-spike](../operations/runbooks/ai-cost-spike.md), [tenant-security-incident](../operations/runbooks/tenant-security-incident.md).
