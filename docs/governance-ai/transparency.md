# AI Transparency

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + Frontend |
| Blueprint | BP §16.2 |

## Requirements (BP §16.2) and Realisation

| Requirement | Realisation |
|---|---|
| AI-generated and human-edited sections distinguishable when relevant | `section_origins` per version; editor toggle "show AI/human origin"; version history labels (AI draft, human edit, system) |
| Ambiguous information displays confidence warnings | ConfidenceBadge (medium/low), warnings panel, review items |
| Important facts expose an Evidence Viewer | Evidence indicators on bound fields |
| Users understand output is Draft until confirmation/approval | Draft banner on all non-final artifacts; exports of non-final artifacts watermarked "Draft" (configurable) |
| AI interaction/content disclosures where required by law/market | Configurable disclosure text in exports (footer "Prepared with AI assistance and reviewed by …") and in-app notice; per-organization toggle subject to jurisdiction |

## Admin Transparency

AI Policy screen shows: which provider categories may process data (per vendor registry), payload retention settings, training/evaluation consent state, human-review defaults (BP App. A "AI Policy").
