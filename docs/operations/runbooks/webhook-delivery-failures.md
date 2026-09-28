# Runbook: Webhook Delivery Failures

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE + Integrations |

**Symptoms:** customers report missing webhooks; deliveries piling up; endpoints auto-disabled.

**Detection:** delivery failure rate alert; backlog growth; `webhook.delivery.failed` events.

**Immediate actions:** distinguish platform-side (egress proxy, DNS, worker backlog) from customer-side (4xx/5xx/timeouts from specific endpoints).

**Diagnosis:** egress proxy health; DNS resolution; TLS errors (customer cert issues); SSRF guard blocking (endpoint resolves to private IP after DNS change).

**Mitigation:** platform issue → fix and replay deliveries from `webhook_deliveries` (idempotent by event ID); customer issue → notify org admins with failure details; re-enable endpoint after fix.

**Recovery/Validation:** backlog drained; success rate normal.

**Escalation:** Integrations owner.

**Post-incident:** improve failure visibility in Integrations UI.
