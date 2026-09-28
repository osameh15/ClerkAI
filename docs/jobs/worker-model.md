# Worker Model

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Processing domain + SRE |
| Blueprint | BP §6 (Worker Pool: Audio, Vision, Document, AI Gateway, Export) |

## Pools

| Pool (task queue) | BP §6 mapping | Isolation | Scaling signal |
|---|---|---|---|
| `general` | Orchestration, knowledge, artifacts | Standard | Workflow task latency |
| `media` | Audio, Video | Temp disk, CPU quota | Schedule-to-start latency, queue depth |
| `document` | Document, Vision preprocessing, scanning calls | Sandbox, no egress | Queue depth |
| `ai` | AI Gateway | Egress to allowlisted providers only | Provider limits, latency |
| `export` | Export | Sandbox, no egress | Queue depth |
| `webhook` | Integrations | Egress via SSRF-guarded proxy | Delivery backlog |

## Rules

- Same Go binary, queue selection by flag; one pool can host several queues in small deployments.
- Workers are stateless; temp files in per-activity directories removed on completion/failure.
- Workers never hold credentials beyond their pool's needs (for example only `ai` has provider credentials; only `export` has write access to exports bucket).
- Graceful shutdown lets activities heartbeat-timeout to other workers.
- Native module steps (official only) run on the queue their step type declares.
