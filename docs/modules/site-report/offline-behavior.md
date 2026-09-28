# Site Report — Offline Behaviour

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (device encryption Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend + Product |
| Blueprint | BP §3.2 (Offline capture), §21, §27 ("Limited queue + explicit status in Site MVP; evolve sync sophistication gradually"), [PDR-007](../../governance/pdr/PDR-007-site-report-mobile-offline.md) |

## What Works Offline (v1)

| Action | Offline |
|---|---|
| Open the Site Report card and previously loaded projects | ✓ (cached) |
| Create a visit (draft job) | ✓ (local draft with temp ID `tmp_…`) |
| Capture photos, video, voice, text notes into observation groups | ✓ |
| Add titles/notes to observations, manual grouping | ✓ |
| View previous visit's open items (if pre-cached) | ✓ (read-only snapshot, cached when online) |
| Submit the visit | Queued: submission happens after all captures synced |
| AI processing, review, report editing, approval, export | ✗ (online) |

## Sync Model

- Local entities: `LocalVisit`, `LocalObservationGroup`, `LocalCapture` (blob + metadata + idempotency keys) in IndexedDB ([indexeddb-model](../../pwa/indexeddb-model.md)).
- When online: create server draft job (idempotent via stored key) → upload sessions per capture (multipart, resumable) → attach to job slots → mark synced → (if user pressed Submit offline) submit.
- Statuses per capture: **Local → Pending → Uploading → Synced** (+ `Failed – retry`) shown explicitly (BP §21).
- Local files are deleted only after server confirms upload **and** SHA-256 integrity (BP §21) and the job references the Source.

## Conflicts

Offline edits apply only to local drafts owned by the device; the server draft is created from the device. If the same visit is edited from another device after sync, server wins for metadata with a merge prompt ([conflict-resolution](../../pwa/conflict-resolution.md)).

## Storage Limits

Quota estimate via `navigator.storage.estimate()`; warn at 70%, block new video capture at 90% (photos/voice still allowed); request persistent storage.

## Sensitive Data

Device-level protection per [OD-023](../../governance/open-decisions.md#od-023); captures purged locally after sync; sign-out prompts if unsynced data exists (never silently deletes).
