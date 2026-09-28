# User-Facing Terminology

| Field | Value |
|---|---|
| Status | Proposed |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product / Design |
| Blueprint | BP §20, App. A |

## Purpose

Maps canonical platform terms ([glossary](../governance/glossary.md)) to the words shown in the UI (English and Persian). Engineering terms that users should not see are listed explicitly.

## UI Vocabulary

| Canonical term | UI label (EN) | UI label (FA, proposed) | Notes |
|---|---|---|---|
| Module | Module / card title | ماژول | Card shows module name, not key |
| Module Store | Module Store | فروشگاه ماژول | Top-level nav |
| Source | Source (Inbox) | منبع | "File" acceptable in upload widget |
| Job | Job | کار | Could be localised per module ("Meeting", "Visit") via manifest `ui.labels` |
| Artifact | Document / output name ("Minutes", "Site Report") | سند | Users rarely see "Artifact" |
| Knowledge item | Item / field | مورد | |
| Evidence | Evidence / "Show source" | شاهد / نمایش منبع | |
| Review Item | Needs review | نیاز به بررسی | |
| Approval | Approval | تأیید | |
| Action Item | Action | اقدام | |
| AI Credit | AI Credits | اعتبار هوش مصنوعی | Never "tokens" ([BDR-002](../governance/bdr/BDR-002-cost-aware-pricing.md)) |
| Organization | Workspace / Organization | فضای کاری / سازمان | Personal = "Personal workspace" |

## Hidden Engineering Terms

Never shown to end users: ProcessingRun, ProcessingStep, AIExecution, Model Profile, Prompt Version, Extraction, Chunk, Outbox, Tenant. Admin/diagnostic screens may show AIExecution metadata to Owners/Admins only where the AI Policy screen requires it.

## Related Documents

[glossary](../governance/glossary.md), [design-system](../frontend/design-system.md)
