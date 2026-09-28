# Disable vs Uninstall

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §4.1 |

> **Disable is not equivalent to Uninstall.** Historical artifacts remain readable according to retention and authorization policy in both cases.

## Comparison

| Aspect | Disable | Uninstall |
|---|---|---|
| Purpose | Temporarily stop new use | Remove the module from the organization |
| New jobs | Blocked | Blocked |
| In-flight jobs | Continue to completion | Cancelled (with confirmation) |
| Launcher card | Hidden for creators (admins see "disabled") | Removed |
| Configuration | Kept, editable | Kept (read-only, restored on reinstall if same major) |
| Permissions | Remain registered | Deactivated (kept in catalogue for audit interpretation; hidden from role editor) |
| Templates/workflows registered | Kept | Deactivated; org-customised templates retained for historical exports |
| Artifacts, Knowledge, Jobs | Readable/editable per policy | Readable (read-only) per retention and authorization |
| Exports of historical artifacts | Allowed | Allowed (renderers use pinned template versions) |
| Action Items | Active | Active (they are platform objects) |
| Seat/installed-module count | Counts | Does not count |
| Billing | Add-on continues unless cancelled | Add-on cancellation is a separate billing action |
| Reversal | Enable | Reinstall (new installation row; history linked) |

## Data Retention After Uninstall

Uninstall **does not delete** data. Module data follows organization retention policies; a separate, explicit "delete module data" request (Owner, MFA, confirmation) can schedule deletion of the module's jobs/artifacts via the privacy deletion workflow ([deletion](../privacy/deletion.md)).

## Rendering Historical Artifacts

Artifacts keep `content_schema_ref` and template version references; the registry retains old module versions' schemas/templates as long as any artifact references them ([compatibility-policy](compatibility-policy.md)).
