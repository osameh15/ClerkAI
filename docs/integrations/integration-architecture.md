# Integration Architecture

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (v1 scope) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Integrations domain |
| Blueprint | BP §7 (Platform: Integration, Webhook), §8 ("External Integrations: Webhooks + Scoped API Keys"), §5.2 (API and automation volume), App. A (Integrations) |

## v1 Integration Surface

| Mechanism | Direction | Doc |
|---|---|---|
| Public REST API (subset of v1) with scoped API keys | Inbound | [public-api](public-api.md), [api-keys](api-keys.md) |
| Webhooks (signed events) | Outbound | [webhooks](webhooks.md) |
| Email notifications | Outbound | [notification-architecture](../platform/notifications/notification-architecture.md) |

## Future

Connectors (calendar, storage drives, task tools, e-signature) — [future-connectors](future-connectors.md).

## Principles

Integrations use the same authorization and tenancy model; integration traffic is metered (`api` meter) and rate-limited; entitlement `api.access` / `api.webhooks` gates.
