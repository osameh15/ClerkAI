# PDR-007: Site Report Is Mobile/PWA-First and Offline-Aware

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Product |
| Blueprint | BP §3.2, §21, §27, §28.2 |

## Decision

Site Report capture is designed for phones on job sites with unreliable networks: offline capture of photos, voice and notes; local upload queue; resumable transfer; explicit Local / Pending / Uploading / Synced status; local files retained until server confirms upload and integrity. Offline synchronisation is designed now, even if the earliest sprint enables only a limited queue (BP §27 "Offline complexity").

## Related

[offline-first](../../pwa/offline-first.md), [offline-site-report](../../pwa/offline-site-report.md), [site-report/offline-behavior](../../modules/site-report/offline-behavior.md)
