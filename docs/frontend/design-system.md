# Design System

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline (UI kit Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product Designer + Frontend Lead |
| Blueprint | BP §20, §20.2, App. A ("baseline for Figma design, frontend route mapping, and component decomposition") |

## Foundations

| Foundation | Rule |
|---|---|
| Tokens | Colour (semantic: primary, surface, success, warning, danger, confidence-high/medium/low), spacing, radius, typography, elevation, motion; dark mode ready |
| Typography | Persian-first font stack (for example Vazirmatn) + Latin pairing; numeric shaping options (Persian/Latin digits) |
| Direction | Logical properties (`margin-inline-start`); mirrored icons where directional |
| Accessibility | WCAG 2.2 AA contrast; focus rings; reduced motion; screen-reader labels in fa/en |
| Module accents | Manifest `ui.card.accent` refers to token names only (no raw colours from modules) |
| Status language | Consistent chips for Job, Artifact, Action, Installation statuses |

## Components

Blueprint component list (BP §20.2) and specs: [component-catalog](component-catalog.md). UI kit base: [OD-014](../governance/open-decisions.md#od-014).
