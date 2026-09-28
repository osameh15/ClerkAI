# Offline Site Report (PWA Implementation)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend Lead |
| Blueprint | BP §3.2, §21 |

Module behaviour: [site-report/offline-behavior](../modules/site-report/offline-behavior.md).

## Pre-Caching When Online

On opening a project (or pinning it for offline), the PWA caches: project metadata, location vocabulary, module configuration (categories, severities), previous final visit's open items with thumbnails (before photos), current user permissions hints.

## Capture Pipeline on Device

1. Start visit → `local_jobs` record.
2. "New observation" → `capture_groups` record; photos (CameraCapture), voice (AudioRecorder), note.
3. Each capture: compress photo (configurable max resolution, preserve EXIF in original if allowed), compute SHA-256, store blob, status Local.
4. Previous items checklist: "after" photo linked explicitly to previous issue ID (stored in capture metadata → used by repeat inspection).

## Storage Limits

| Threshold (of quota estimate) | Action |
|---|---|
| 70% | Warning banner |
| 90% | Block video capture; allow photo/voice/text |
| 97% | Block new captures; prompt to sync or free space |

## Encryption Considerations

Options under [OD-023](../governance/open-decisions.md#od-023): rely on device encryption + purge after sync (baseline); optional Web Crypto encryption of blobs with a session-derived key (loses data if session key lost → requires careful UX).

## Cleanup

Synced blobs purged; cached reads expire after 7 days; sign-out with unsynced items requires explicit confirmation.
