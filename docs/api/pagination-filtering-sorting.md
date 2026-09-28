# Pagination, Filtering and Sorting

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | BP App. B (list endpoints) |

## Pagination

- **Cursor-based** only. Request: `?limit=25&cursor=<opaque>`; `limit` 1–100, default 25.
- Response envelope: `{ "data": [...], "page": { "next_cursor": "…" | null, "has_more": true|false } }`.
- Cursors are opaque, base64url-encoded, **signed** (HMAC) server-side values containing the sort key tuple and filter hash. A cursor used with different filters returns `400 VALIDATION_CURSOR_INVALID`.
- Stable ordering: every sort is suffixed with `id` as a tie-breaker (UUIDv7 is time-ordered).
- No total counts by default (expensive under RLS); endpoints that need counts expose explicit `*_count` fields or summary endpoints.

## Filtering

- Simple equality filters as named query parameters (`status=processing`, `module_key=…`, `project_id=…`).
- Multi-value: repeat the parameter (`status=queued&status=processing`).
- Time ranges: `from`, `to` (RFC 3339), inclusive-exclusive.
- Full-text: `q` where documented ([OD-031](../governance/open-decisions.md#od-031)).
- Filters are allowlisted per endpoint in OpenAPI; unknown filters → `400 VALIDATION_UNKNOWN_PARAMETER`.

## Sorting

- `sort=-created_at` (descending with `-`), comma-separated, allowlisted fields per endpoint. Default `-created_at`.
- Allowed fields must be backed by an index ([indexing-strategy](../data/indexing-strategy.md)).
