# Background Sync

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend Lead |
| Blueprint | BP §21 ("Background sync where supported, with manual retry fallback") |

## Strategy

| Environment | Behaviour |
|---|---|
| Chromium-based (Android) | Background Sync API registration on queue changes; Periodic Background Sync (if granted) for retries |
| Safari/iOS | No reliable background sync → sync on app open/foreground, visibility change, and "Sync now" button; reminders to open app |
| All | Service worker cannot hold large uploads reliably → background task triggers sync engine which resumes multipart in small steps |

## UX

Persistent sync chip with count of pending items; local notification (where permitted) when uploads complete or fail; warning when items have been pending longer than `offlineRetentionDays`.

Feature flag: `pwa.background-sync` for rollout.
