# Evidence Display in Artifacts

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Artifacts domain + Frontend |
| Blueprint | BP §10, §16.2 ("Important facts expose an Evidence Viewer"), §20.2 (EvidenceViewer) |

## In the Web Editor/Viewer

- Bound fields show an evidence indicator (count, strongest pointer type). Click → Evidence Viewer opens the source at the pointer: audio/video seek to time range with waveform highlight; PDF/image page with bounding box overlay; text with span highlight ([evidence-ui](../frontend/evidence-ui.md)).
- Contradicting evidence is shown distinctly.
- `source_unavailable` evidence shows metadata (source title, location) with "source deleted per retention policy".

## In Exports

Exports are standalone documents; evidence is represented (configurable per template):

| Option | Rendering |
|---|---|
| `none` (default for formal minutes/letters) | No evidence markers |
| `references` | Footnote-style references: "Recording 00:21:12; Notes p.2" |
| `appendix` | Evidence appendix with quotes/thumbnails (Site Report photo log) |

Exports never embed links that bypass authorization; optional deep links require sign-in.
