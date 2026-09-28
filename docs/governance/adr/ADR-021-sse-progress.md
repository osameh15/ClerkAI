# ADR-021: SSE for Processing Updates; WebSocket Only When Needed

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09-28 |
| Deciders | Technical Leadership, Frontend Lead |
| Blueprint | BP §6, §8 |

## Decision

Job progress is streamed with **Server-Sent Events** at `GET /v1/jobs/{id}/events`. Events carry monotonically increasing IDs; clients reconnect with `Last-Event-ID` and receive missed events from persisted ProcessingSteps/Job state. Polling the Job resource is the fallback. WebSocket is reserved for future live collaboration (BP §8) and requires a new ADR.

## Related

[api/sse](../../api/sse.md), [processing-status-ui](../../frontend/processing-status-ui.md)
