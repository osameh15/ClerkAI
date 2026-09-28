# ADR-023: Go HTTP Router = go-chi/chi v5

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09-28 |
| Deciders | Technical Leadership |
| Blueprint | BP §6, §8 |
| Closes | [OD-001](../open-decisions.md#od-001) |

## Context

The API needs routing, middleware composition (request ID, tracing, auth, CSRF, tenant resolution, rate limiting, idempotency) and compatibility with `net/http` so that OpenAPI-generated types and OpenTelemetry instrumentation plug in without a framework owning the request context ([ADR-004](ADR-004-rest-openapi.md)).

## Decision

Use **`github.com/go-chi/chi/v5`** as the HTTP router for `apps/api`.

## Constraints

1. chi is an **HTTP transport concern only**. It may be imported only in `apps/api` and `internal/<context>/adapters/http` packages (and `internal/platform` HTTP middleware helpers). It must never be imported by `domain`, `app` or `ports` packages.
2. Handlers use standard `http.Handler` / `http.HandlerFunc`; request data is decoded into OpenAPI-generated request types and mapped to use-case commands; no chi types cross into use cases.
3. Route parameters are read in the adapter and passed as typed values (for example `ids.JobID`).
4. Enforcement: the import-boundary lint ([dependency-rules](../../architecture/dependency-rules.md)) forbids `github.com/go-chi/*` outside the allowed packages.

## Consequences

- Positive: idiomatic `net/http`, small dependency, rich middleware ecosystem, easy integration with `oapi-codegen` (chi server target) and `otelhttp`.
- Negative: no built-in validation/binding — handled by generated types + JSON Schema validation middleware (intended).

## Alternatives Considered

Plain `net/http` 1.22+ routing (viable, fewer middleware conveniences); `echo`/`gin` (own their context types, higher risk of leaking into logic).
