# Brand Kit

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Templates domain |
| Blueprint | BP §12 ("Brand Kit: Logo, fonts, header/footer, colors, signatures"), App. B (`/v1/brand-kit`) |

## Contents

| Element | Constraints |
|---|---|
| Logo | Uploaded as a Source (image), validated, rendition stored; SVG sanitised or rasterised |
| Fonts | From an allowlisted font catalogue (including Persian fonts such as Vazirmatn) or uploaded font files (TTF/OTF, licence acknowledgement, validated); embedded in PDF, referenced/embedded in DOCX |
| Colours | Primary/secondary/accent as design tokens; contrast checked for accessibility |
| Header/Footer | Slot templates: org name, logo, document number, date, page x/y, confidentiality label |
| Signatures | Named signatory blocks (name, title, optional signature image) usable by approval stages |
| Page numbering | Style (Persian/Latin digits), position |
| Document numbering | References numbering sequences ([numbering-sequences](numbering-sequences.md)) |

## Versioning

Each update creates a new brand kit version row (`is_current`); exports reference the version used, so historical exports are reproducible.

## Entitlement

Custom brand requires `template.custom` (BP App. C: Personal —; Team+ ✓). Without it, a neutral platform brand is used.
