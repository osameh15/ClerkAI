# Metadata Extraction

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Sources domain |
| Blueprint | BP §9, §3.2 (location), §15 (privacy) |

## Extracted Metadata

| Kind | Fields |
|---|---|
| All | detected MIME, size, SHA-256, creation/modified times from container |
| Audio/Video | duration, codecs, sample rate, channels, resolution, frame rate, rotation |
| Image | dimensions, orientation, EXIF capture time, GPS (lat/lng/accuracy), device model |
| PDF | page count, text-layer ratio, producer, encryption flag |
| Office | page/slide/sheet counts, author metadata (stored, not displayed by default) |

## Privacy Rules

- Device model and author metadata are stored for evidence/provenance but classified Restricted and not surfaced to other users by default.
- GPS used only when the module needs location (Site Report) and the capture UX obtained permission; otherwise dropped at extraction.
- Client-provided capture metadata (`captured_at`, `capture_location` from the PWA) takes precedence over EXIF when both exist and is labelled as such.
