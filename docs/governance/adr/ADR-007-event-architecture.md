# ADR-007: Events = Transactional Outbox + Event Bus Abstraction

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership |
| Blueprint | BP §8, §8.2, §8.3, §28.1 |

## Context

Contexts must react to each other's facts (usage recording, audit projection, notifications, SSE, webhooks) without direct coupling, and without "dual write" inconsistency between the database and a broker.

## Decision

1. Domain events are written to an `outbox_events` table **in the same transaction** as the state change.
2. A **dispatcher** reads the outbox in order and publishes through an `EventBus` port.
3. Consumers are idempotent; they record processed `event_id`s (inbox pattern) per consumer.
4. Events use the versioned envelope ([event-envelope.schema.json](../../api/schemas/event-envelope.schema.json)) and naming `<domain>.<entity>.<event>` (entity segment omitted when it equals the domain aggregate, for example `job.submitted`).
5. Domain code depends only on the `EventBus`/`Outbox` ports — never on a specific broker.
6. The **broker choice is open** ([OD-007](../open-decisions.md#od-007)). v1 baseline: in-process dispatcher delivering to registered consumers; external broker (for example NATS JetStream) can be added behind the port.

Events are *notifications of facts*, not commands and not a system of record. Orchestration belongs to Temporal ([ADR-003](ADR-003-temporal-workflows.md)).

## Consequences

- At-least-once delivery; consumers must handle duplicates and out-of-order delivery across aggregates.
- Outbox table requires cleanup/partitioning policy ([partitioning-strategy](../../data/partitioning-strategy.md)).

## Related

[event-architecture](../../architecture/event-architecture.md), [idempotency-strategy](../../architecture/idempotency-strategy.md)
