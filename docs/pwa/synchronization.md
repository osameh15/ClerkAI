# Synchronization

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend Lead |
| Blueprint | BP §21 |

## Offline Synchronization Flow

```mermaid
sequenceDiagram
  autonumber
  participant U as Inspector
  participant P as PWA (IndexedDB)
  participant SE as Sync engine
  participant API as clerk-api
  participant S3 as Object storage
  U->>P: create visit (tmp_job) + captures (tmp_cap) offline
  Note over P: status Local
  SE->>SE: online detected / manual "Sync now"
  SE->>API: POST /jobs {module_key, project_id, client_ref: tmp_job} (Idempotency-Key K1)
  API-->>SE: 201 job_id (or replayed response)
  SE->>P: map tmp_job → job_id
  loop each capture (bounded concurrency)
    SE->>API: POST /uploads {…, client_ref: tmp_cap, job_id, job_slot} (K2)
    API-->>SE: session + URLs
    SE->>S3: PUT parts (resume on failure)
    SE->>API: POST /uploads/{id}/complete {sha256} (K3)
    API-->>SE: Source (scanning) attached to job slot
    SE->>P: tmp_cap → source_id, status Synced
  end
  alt user pressed Submit while offline
    SE->>API: POST /jobs/{id}/submit (K4, created at press time)
  end
  SE->>P: purge synced blobs
```

## Rules

- Commands are replayed in dependency order from the IndexedDB outbox; idempotency keys make replays safe across app restarts.
- Server-generated state (job status, artifacts) is pulled after sync; local copies of server entities are caches only.
- Sync runs in foreground with visible progress; background sync where supported ([background-sync](background-sync.md)).
- Session expiry during sync pauses the engine and asks for re-authentication; no data lost.
