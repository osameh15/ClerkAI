# IndexedDB Model

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend Lead |
| Blueprint | BP §21 ("IndexedDB for draft metadata and manageable blobs") |

## Database `clerk-offline` (versioned schema, migrations on upgrade)

| Store | Key | Fields | Indexes |
|---|---|---|---|
| `meta` | key | schema version, user id, org ids, last sync | — |
| `local_jobs` | `local_id` (`tmp_…`) | org_id, module_key, project_id, title, input, server_job_id?, status (`local`, `syncing`, `synced`, `failed`), submit_requested, idempotency keys (create/submit), created_at | org_id, status |
| `capture_groups` | `local_id` | local_job_id, created_at, location, note, order | local_job_id |
| `captures` | `local_id` | local_job_id, capture_group_id, slot, kind, mime, size, sha256, captured_at, gps?, blob_ref, sync_status (`local`, `pending`, `uploading`, `synced`, `failed`), upload_session_id?, completed_parts[], server_source_id?, idempotency keys, error | local_job_id, sync_status |
| `blobs` | `blob_ref` | Blob (or chunked parts for large media) | — |
| `recording_chunks` | `recording_id + seq` | audio/video chunks during live recording | recording_id |
| `cached_reads` | `cache_key` | org-scoped cached API responses (module cards, projects, previous visit open items) with ETag and fetched_at | org_id |
| `outbox` | `seq` | pending commands (create job, attach, submit, resolve action) with idempotency key and dependencies | status |

## Rules

- All records include `org_id` and `user_id`; data for other users on shared devices is not readable after sign-out (cleared or encrypted per [OD-023](../governance/open-decisions.md#od-023)).
- Blobs stored as `Blob`; large videos chunked (for example 8 MB) aligned with multipart part size.
- Storage quota monitored ([offline-site-report](offline-site-report.md)).
