# MIME Validation

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Sources domain + Security |
| Blueprint | BP §9.1 ("MIME sniffing; never trust the client-provided Content-Type alone"), §14.1 (Malicious file) |

## Rules

1. Declared `content_type` + extension are checked against the allowlist at session creation (fast rejection).
2. After upload, the **sniffed** type (magic bytes + container inspection) is authoritative. Mismatch between declared and sniffed kind → `rejected` (`UPLOAD_TYPE_NOT_ALLOWED`), unless both map to the same source kind (for example `audio/x-m4a` vs `audio/mp4`).
3. Container validation: ZIP-based Office files are checked for zip bombs (entry count, compression ratio, total uncompressed size), macros (`.docm`/`.xlsm` rejected), external relationships (stripped/ignored).
4. PDFs: JavaScript, launch actions, embedded files are ignored during parsing; encrypted PDFs rejected unless password flow is added later.
5. Images: decoded with size limits (megapixels) to prevent decompression bombs; HEIC converted to rendition JPEG.

## v1 Allowlist

| Kind | MIME types |
|---|---|
| audio | `audio/mpeg`, `audio/mp4`, `audio/x-m4a`, `audio/aac`, `audio/wav`, `audio/x-wav`, `audio/ogg`, `audio/webm`, `audio/flac`, `audio/amr` |
| video | `video/mp4`, `video/quicktime`, `video/webm`, `video/x-matroska` |
| image | `image/jpeg`, `image/png`, `image/webp`, `image/heic`, `image/heif`, `image/tiff` |
| document | `application/pdf`, `application/vnd.openxmlformats-officedocument.wordprocessingml.document` |
| presentation | `application/vnd.openxmlformats-officedocument.presentationml.presentation`, PDF slides |
| spreadsheet | `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`, `text/csv` |
| text | `text/plain`, `text/markdown` |

Legacy binary Office formats (`.doc`, `.ppt`, `.xls`) are not supported in v1 (parser attack surface); users are asked to convert.
