# Responsive Design

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend + Product Design |
| Blueprint | BP §21 (mobile-first), §20 |

## Breakpoints (tokens)

`sm < 640px` (phones), `md 640–1024` (tablets), `lg > 1024` (desktop). Mobile-first CSS.

## Priorities by Device

| Flow | Phone | Tablet | Desktop |
|---|---|---|---|
| Site capture | Primary | Good | Rare |
| Review queue | Good (card per item) | Good | Primary (keyboard) |
| Artifact editing | Basic (single column) | Good | Primary (three panels) |
| Admin/settings | Functional | Good | Primary |

## Field Conditions (Site Report)

Touch targets ≥ 48 dp; high-contrast mode; one-handed reachable primary actions (bottom); minimal typing (voice-first); works with gloves where possible (large targets); brightness-friendly palette.
