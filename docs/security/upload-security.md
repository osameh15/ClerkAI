# Upload Security

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security + Sources |
| Blueprint | BP §9.1, §14.1 (Malicious file), §14.2 |

| Control | Implementation |
|---|---|
| Presigned URL scope | Single staging key, PUT only, content-type and content-length bound, TTL ≤ 15 min |
| Size limits | Per plan/module/platform ceilings enforced at session creation and verified at completion |
| Type validation | Allowlist + magic-byte sniffing; mismatch rejected ([mime-validation](../ingestion/mime-validation.md)) |
| Malware scan | Before any parsing ([malware-protection](malware-protection.md)) |
| Parser isolation | Sandboxed workers (no network, seccomp/AppArmor or gVisor-class sandbox where available, CPU/memory/time limits, read-only FS except temp) |
| Decompression bombs | Ratio and size caps for ZIP/Office, image pixel caps, PDF object count caps |
| Filename safety | Filenames never used as keys or paths; sanitised for display; `Content-Disposition` encoded |
| Serving files | Only via presigned GET with `Content-Disposition: attachment` for downloads; inline previews only for safe renditions (images, PDF via viewer) with `X-Content-Type-Options: nosniff` |
| EXIF/GPS | Extracted and stripped from shared renditions |
| Abuse | Per-user/org upload rate limits and storage quotas |
