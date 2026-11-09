---
id: "02-aura-ash-exo/aura-wm-core"
title: "Aura window-management core"
kind: "collection"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile", "evidence:source"]
sources: [S-1f554a1713a5, S-540f3290d528, S-697de35f64e0, S-a91a3b4fc798, S-bde2848501a4, S-f38448d02e6c, S-514bff6c69a0]
---

# Aura window-management core

Aura windows, transient parenting, modality, capture and focus are engine mechanisms; Ash policies that call them are separate donors. [docs]

## What Chromium already has

The [row data](rows/aura-lift.tsv) holds 34 rows for this beat: ADAPT 10, FRESH 5, LIFT 19. These are historical source verdicts. [docs]

Donor availability in those rows: `in-engine-build` 34. Zero Ash references in a file do not bypass its target build gate. [docs]

- Aura retains child stacking primitives independently of the shell policy that chooses their order. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window.h#L405).
- WindowModalityController mediates event delivery for modal transient windows; the shell still determines desktop-specific placement and membership. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/window_modality_controller.h#L30).
- HostFrameRateThrottler sets a twenty-frame-per-second cap outside Windows; that cap alone does not establish zero work for an idle compositor. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/host_frame_rate_throttler.cc#L15), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/host_frame_rate_throttler.cc#L45).
- Aura already declares a typed attention-request property; a historical claim that urgency always requires a new property is broader than the checked source supports. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/client/aura_constants.h#L93).

## Domicile at the pin

- `engine:aura`: Aura is already part of the Linux engine; the pinned architecture keeps native client delivery in Rust and embeds its surfaces through Chromium. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md#L226).

## Boundary

A transient-parent relationship and a modal event filter describe different invariants. Page layout, focus routing and protocol state retain separate ownership. [inferred]

## Feature links

- [aura-wm/window-states](../06-features/aura-wm/window-states.md)
- [shell-surfaces/popup-and-popover-boundaries](../06-features/shell-surfaces/popup-and-popover-boundaries.md)

## Sources

- S-a91a3b4fc798 · 2026-07-18 · Chromium aura window-management core source and equivalence inventory
- S-1f554a1713a5 · 2026-07-18 · Lift-map evaluation and corrections
- S-540f3290d528 · 2026-11-09 · Pinned donor interface inspection
- S-697de35f64e0 · 2026-11-09 · Pinned donor interface inspection
- S-bde2848501a4 · 2026-11-09 · Pinned donor interface inspection
- S-f38448d02e6c · 2026-11-09 · Pinned donor interface inspection
- S-514bff6c69a0 · 2026-11-09 pin check · Domicile engine integration and patch discipline
