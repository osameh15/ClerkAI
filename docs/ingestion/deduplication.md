# Deduplication

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Sources domain |
| Blueprint | BP §9.1 ("SHA-256 for integrity and tenant-level deduplication"), §22.1 |

## Behaviour

1. At completion, the verified SHA-256 is looked up in `sources(organization_id, sha256)` among non-deleted Sources.
2. If a match exists, a **new Source row is still created** (so the user's inbox, titles, project and access scopes stay independent), but:
   - its original `source_files` row references the **same object key** (reference-counted), and the staged upload is deleted;
   - extractions are linked by reference ([extraction-cache](extraction-cache.md));
   - storage usage is not double-counted for the organization.
3. The user is informed ("This file was already uploaded on … — reused processing").

## Reference Counting

Objects are deleted only when no `source_files` row references them. Deletion workflows decrement references transactionally.

## Why Not Cross-Tenant

Cross-tenant dedup leaks existence ("someone else uploaded this exact file") and couples retention across tenants. Forbidden.

## Near-Duplicates

Perceptual/audio fingerprint dedup is Future; not in v1.
