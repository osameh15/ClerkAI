# AI Observability

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI domain + SRE |
| Blueprint | BP §17, §16.1, §22.1 (anomaly detection) |

## Telemetry

| Signal | Content |
|---|---|
| Spans | `ai.execute` span per gateway call; child span per attempt with attributes: `ai_execution_id`, `capability`, `profile`, `provider`, `model`, `prompt_version`, `attempt_kind`, `status`, token/second counts, `cost_micros`, `organization_id` (hashed where exported externally) |
| Metrics | Calls, errors by class, latency histograms, schema-invalid rate, repair rate, fallback rate, cost per minute by provider/model/module, circuit-breaker state, budget rejections |
| Logs | Structured, no prompt/response content |
| Business dashboards | Cost per job/module/org, schema validity, review rates, acceptance/correction rates by prompt version |

## Alerts (provisional)

| Alert | Condition |
|---|---|
| Provider degraded | Error rate > 20% for 5 min on a route |
| Schema validity drop | < 98% over 1 h for any task |
| Cost spike | Org or global AI cost > 3× trailing 7-day hourly baseline ([ai-cost-spike runbook](../operations/runbooks/ai-cost-spike.md)) |
| Budget exhaustion | Org reaches 80%/100% of AI budget (customer notification) |
| Quality regression in production | Human correction rate for a prompt version > baseline + threshold over 7 days |

## Production Quality Monitoring

Sampled production outputs (consent-permitting) are reviewed by humans monthly per module; correction and acceptance rates tracked per prompt version ([human-evaluation](evaluation/human-evaluation.md)).
