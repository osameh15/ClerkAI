# Artifact Editor

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (editor library Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend |
| Blueprint | BP §20.1 (Universal Artifact Editor), App. A |

Contract with backend: [editor-contract](../artifacts/editor-contract.md).

## Layout (BP §20.1)

```text
┌───────────────┬─────────────────────────────┬────────────────┐
│ Sources       │ Artifact Editor             │ AI / Review    │
│ audio (seek)  │ structured sections         │ warnings       │
│ image (bbox)  │ editable fields / tables    │ evidence       │
│ PDF (page)    │ versioned content           │ confidence     │
└───────────────┴─────────────────────────────┴────────────────┘
```

Mobile: single column with bottom sheets for Sources and AI/Review panels.

## Features

- Schema-driven sections (from artifact schema + `x-ui`); hybrid section renderers via extensions.
- Inline ConfidenceBadge and evidence indicators on bound fields; clicking evidence focuses the Sources panel at the pointer.
- Autosave (debounced PATCH with If-Match), save status indicator, conflict resolution UI.
- Version history drawer: list with origin (AI/Human/System), diff view, restore-as-new-version.
- AI/human origin highlighting toggle ([transparency](../governance-ai/transparency.md)).
- Status bar: Draft/In review/Changes requested/Approved/Final; actions: submit for approval, finalize, export, share (permission/entitlement-gated).
- Read-only modes per status; clear explanation when locked.
- Keyboard shortcuts; RTL support; spellcheck (browser) for Persian/English.
