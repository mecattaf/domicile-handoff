---
id: "02-aura-ash-exo/ash-sysui-blur-wallpaper"
title: "Ash system UI, shields and wallpaper"
kind: "collection"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile", "evidence:source"]
sources: [S-0e3d3bee5e8f, S-1f554a1713a5, S-3c38ec8fb4c4, S-5b0a70980a2f, S-5f2e276eeb55, S-ce1db11e9725, S-514bff6c69a0]
---

# Ash system UI, shields and wallpaper

Ash background shields and wallpaper controllers coordinate layer effects, lifecycle and ChromeOS policy. Their engine primitives have separate availability. [docs]

## What Chromium already has

The [row data](rows/aura-lift.tsv) holds 12 rows for this beat: ADAPT 3, LIFT 9. These are historical source verdicts. [docs]

Donor availability in those rows: `chromeos-gated` 12. Zero Ash references in a file do not bypass its target build gate. [docs]

- The system style constants remain sigma thirty and quality one third, approximately; they are defaults, with no measured cost implied. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/style/style_util.h#L51).
- BlurredBackgroundShield owns a background layer below a View and resolves its fill through the host color provider before setting blur and quality. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/style/blurred_background_shield.h#L23), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/style/blurred_background_shield.cc#L104).
- The current system decoration API offers shadows and highlight borders; historical SystemShadow paths are absent at this engine pin. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/style/system_decoration.h#L42).

## Domicile at the pin

- `engine:blink and engine:viz`: Native windows enter the page scene through surface layers; the page can style its own elements while backdrop presentation remains a documented qualification limit. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md#L3).

## Boundary

Layer blur is an engine mechanism; wallpaper source, state and shell policy are separate. [inferred]

## Feature links

- [effects/backdrop-blur](../06-features/effects/backdrop-blur.md)
- [effects/corner-radius-shadows](../06-features/effects/corner-radius-shadows.md)

## Sources

- S-5f2e276eeb55 · 2026-07-18 · Chromium ash system ui, shields and wallpaper source and equivalence inventory
- S-1f554a1713a5 · 2026-07-18 · Lift-map evaluation and corrections
- S-0e3d3bee5e8f · 2026-11-09 · Pinned donor interface inspection
- S-3c38ec8fb4c4 · 2026-11-09 · Pinned donor interface inspection
- S-5b0a70980a2f · 2026-11-09 · Pinned donor interface inspection
- S-ce1db11e9725 · 2026-11-09 · Pinned donor interface inspection
- S-514bff6c69a0 · 2026-11-09 pin check · Domicile engine integration and patch discipline
