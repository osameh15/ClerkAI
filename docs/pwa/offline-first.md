# Offline-First Principles

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend Lead + Product |
| Blueprint | BP §21 ("offline capability a day-one architectural decision… even if every detail is not enabled in the earliest sprint") |

## Principles

| # | Principle |
|---|---|
| OF-1 | Capture never depends on network: writes go to IndexedDB first, then sync. |
| OF-2 | Local data is never deleted until the server confirms upload **and** integrity (SHA-256) (BP §21). |
| OF-3 | Every local item has a visible sync status: Local / Pending / Uploading / Synced (+ Failed). |
| OF-4 | Every server-bound command generated offline carries an idempotency key created at capture time. |
| OF-5 | Temporary local IDs (`tmp_…`) map to server IDs via `client_ref`; references are rewritten after sync. |
| OF-6 | Server remains the system of record; conflicts resolved with explicit rules ([conflict-resolution](conflict-resolution.md)). |
| OF-7 | Sensitive offline data is protected per platform capabilities and purged after sync ([OD-023](../governance/open-decisions.md#od-023)). |
| OF-8 | Sign-out and org switch never silently discard unsynced data. |

## Incremental Delivery (BP §27 "Offline complexity")

| Stage | Capability |
|---|---|
| Sprint 1–G10 | Architecture + data model in place; online-first uploads use the same queue abstraction |
| G11 Site Report MVP | Limited queue + explicit status; manual retry; foreground sync |
| Post-launch | Background sync where supported, smarter conflict handling, offline review |
