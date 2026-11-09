---
id: "02-aura-ash-exo/compositor-blur-anim"
title: "Compositor blur and animation"
kind: "collection"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile", "evidence:source"]
sources: [S-1f554a1713a5, S-6b5cc0eed377, S-7e62b17cb4f1, S-b684634e239b, S-de6569bcc761, S-502e32dd38c7, S-703c0d408b29]
---

# Compositor blur and animation

Aura layers supply filter properties and animation machinery. Those APIs differ from the Ash policy that chooses when and where effects apply. [docs]

## What Chromium already has

The [row data](rows/aura-lift.tsv) holds 19 rows for this beat: ADAPT 6, FRESH 5, LIFT 8. These are historical source verdicts. [docs]

Donor availability in those rows: `in-engine-build` 19. Zero Ash references in a file do not bypass its target build gate. [docs]

- Layer background blur affects content behind a layer; its foreground blur API affects the layer itself. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.h#L266), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.h#L276).
- Backdrop filter bounds use layer-local coordinates and constrain both the effect region and the content available for sampling. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.h#L344).
- The compositor callback observer tracks successful and aborted sequences separately; its ended callback runs after all sequences complete or detach, once activation permits callbacks. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/callback_layer_animation_observer.cc#L44), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/callback_layer_animation_observer.cc#L114).
- Layer animation sequences request redraws before completion notifications because observers may delete their delegate; progression also checks weak pointers after element callbacks. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer_animation_sequence.cc#L82), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer_animation_sequence.cc#L181).

## Domicile at the pin

- `engine:blink and engine:viz`: The native app element participates in CSS compositing. Published foreground-filter comparisons do not settle backdrop sampling and overlay behavior on a lit display. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L228).
- `manganese`: Motion state retains departing windows until their animation ends, and an overview transition can reverse from its current transform. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindowMotion.ts#L171).
- Public changes: [overview #1931](https://github.com/cprussin/domicile/pull/1931), [overview transition #1940](https://github.com/cprussin/domicile/pull/1940), [closing motion #1910](https://github.com/cprussin/domicile/pull/1910), [closing lifetime #1939](https://github.com/cprussin/domicile/pull/1939), and [workspace slide #1942](https://github.com/cprussin/domicile/pull/1942). [docs]

## Boundary

Effect sampling, animation lifetime and physical presentation require separate evidence. A foreground blur cannot stand in for a background-sampling check. [inferred]

## Feature links

- [effects/backdrop-blur](../06-features/effects/backdrop-blur.md)
- [effects/window-animations](../06-features/effects/window-animations.md)

## Sources

- S-b684634e239b · 2026-07-18 · Chromium compositor blur and animation source and equivalence inventory
- S-1f554a1713a5 · 2026-07-18 · Lift-map evaluation and corrections
- S-6b5cc0eed377 · 2026-11-09 · Pinned donor interface inspection
- S-7e62b17cb4f1 · 2026-11-09 · Pinned donor interface inspection
- S-de6569bcc761 · 2026-11-09 · Pinned donor interface inspection
- S-502e32dd38c7 · 2026-11-09 pin check · Pinned shell window-motion implementation
- S-703c0d408b29 · 2026-11-09 pin check · Domicile native surface composition and presentation boundaries
