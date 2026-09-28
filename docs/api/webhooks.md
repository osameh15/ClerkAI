# Webhooks (Outbound Contract)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (delivery executor Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Integrations domain |
| Blueprint | BP §8 ("Webhooks + Scoped API Keys"), §8.3, App. B |

## Subscription

`POST /v1/webhooks` with an HTTPS URL and `event_types` (exact types or `domain.*` wildcards) from the public subset of the [event catalogue](../architecture/event-architecture.md#7-event-catalogue-v1-baseline). Entitlement `api.webhooks` required.

## Delivery Request

```http
POST {endpoint_url}
Content-Type: application/json
User-Agent: Clerk-Webhooks/1
Clerk-Webhook-Id: <delivery_id>
Clerk-Event-Id: <event_id>
Clerk-Event-Type: artifact.approved
Clerk-Timestamp: 1790000000
Clerk-Signature: v1=<hex HMAC-SHA256(secret, "{timestamp}.{raw_body}")>
```

Body: the [event envelope](schemas/event-envelope.schema.json) filtered to public `data` fields.

## Semantics

- At-least-once delivery; receivers deduplicate on `Clerk-Event-Id` (BP §8.3).
- Success = any `2xx` within 10 s. Otherwise retry with exponential backoff (1 min, 5 min, 30 min, 2 h, 6 h, 12 h …) for up to 24 h; then the delivery is `dead`.
- After repeated failures (for example 50 consecutive), the endpoint is set `failing` → `disabled`; Owner/Admin notified.
- Delivery/retry history is stored in `webhook_deliveries` and visible to admins (BP §8.3).
- Ordering is not guaranteed; use `occurred_at` and resource fetches.

Security: [webhook-security](webhook-security.md). Execution design: [OD-035](../governance/open-decisions.md#od-035), [integrations/webhooks](../integrations/webhooks.md).
