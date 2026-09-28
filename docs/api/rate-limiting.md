# Rate Limiting

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (limits Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) + Security |
| Blueprint | BP §14.1 (Abuse/cost attack), §14.2 ("Layered rate limits by user, organization, API key, and IP, plus AI budget limits") |

## Layers

| Layer | Key | Default (provisional) | Purpose |
|---|---|---|---|
| Edge / IP | client IP (prefix for IPv6 /64) | 300 req/min; auth endpoints 10/min | Abuse, credential stuffing |
| User | user id | 600 req/min | Runaway clients |
| Organization | org id | 3,000 req/min (plan-scaled) | Tenant fairness |
| API key | key id | 600 req/min (plan-scaled) | Integration control |
| Command-specific | org + command | upload sessions 60/min; job submits 30/min; exports 30/min | Cost protection |
| AI budget | org monthly budget | Per plan / org setting | Cost protection ([quotas](../billing/quotas.md)) |

Algorithm: sliding-window or token bucket in Redis; if Redis is unavailable, a conservative in-process limiter applies ([redis-usage](../data/redis-usage.md)).

## Response

`429 RATE_LIMITED` with `Retry-After` and `RateLimit-*` headers. The error `meta` includes `scope` (`ip`, `user`, `organization`, `api_key`, `command`).

Quota exhaustion (commercial) is not rate limiting: it returns `402 BILLING_QUOTA_EXCEEDED`.
