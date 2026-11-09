---
id: sway-family/swayfx
title: Swayfx effects and compositor boundaries
kind: collection
layers: [external, any-shell]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: beyond
donors: [swayfx, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [bucket:domicile, evidence:docs, scroll-parity:beyond]
sources: [S-292e57b07af7, S-03db46ad9d28, S-e0dd6eebeebc, S-ab41375a77ca, S-de92af3f5a9b, S-73cd44ceeaac, S-c96f22466372, S-d5a816264f96, S-47f30e0b6caa, S-f8f2f5e99161]
---

# Swayfx effects and compositor boundaries

Swayfx adds effect controls and lifecycle changes to sway. The functional contracts concern which surface is affected, how effect state is selected, and what happens during layout changes. The rendering implementation is specific to its scene and renderer stack.

The donor reference is Swayfx 0.6, published 2026-08-05. Its release notes identify the Sway 1.12, wlroots 0.20 and SceneFX 0.5 base. These versioned references describe the donor implementation, separately from the recorded fork validation below. [docs] [Release and base versions](https://github.com/wlrfx/swayfx/releases/tag/0.6)

## Effect contracts

| Control | Observable scope | Pinned donor reference |
|---|---|---|
| Blur | Window enablement can follow criteria; parameters tune blur globally; xray restricts the sampled background. | [swayfx 0.6](https://github.com/wlrfx/swayfx/blob/0.6/sway/sway.5.scd#L748) [docs] |
| Shadows | Global or matched-window enablement, client-decoration handling, inactive state and offset. | [swayfx 0.6](https://github.com/wlrfx/swayfx/blob/0.6/sway/sway.5.scd#L727) [docs] |
| Rounded corners | Radius for new windows; smart mode conditions it on surrounding gaps. | [swayfx 0.6](https://github.com/wlrfx/swayfx/blob/0.6/sway/sway.5.scd#L705) [docs] |
| Dimming | Inactive-window strength and distinct inactive/urgent state. | [swayfx 0.6](https://github.com/wlrfx/swayfx/blob/0.6/sway/sway.5.scd#L712) [docs] |
| Layer surfaces | Namespace matching selects blur, shadow, radius and reset behavior separately from window criteria. | [swayfx 0.6](https://github.com/wlrfx/swayfx/blob/0.6/sway/sway.5.scd#L866) [docs] |
| Scratchpad minimize | When enabled, xdg-shell minimize requests add or hide the containing scratchpad item; foreign-toplevel unminimize shows it. | [client request](https://github.com/wlrfx/swayfx/blob/0.6/sway/desktop/xdg_shell.c#L395) · [foreign-toplevel request](https://github.com/wlrfx/swayfx/blob/0.6/sway/tree/view.c#L801) [docs] |

## Animation lifecycle

At the 0.6 release reference, close animation retains the container until its completion callback removes the saved client buffer and destroys the container. The close transition begins from the currently interpolated size and opacity. [docs] [Retained lifetime](https://github.com/wlrfx/swayfx/blob/0.6/sway/desktop/transaction.c#L94) · [Current-state close](https://github.com/wlrfx/swayfx/blob/0.6/sway/desktop/transaction.c#L170)

Interactive resizing and floating-window dragging finish the running animation and arrange directly. Other moves start from tracked current size, opacity and scene position, compensating for scene reparenting. [docs] [Interactive path and retargeting](https://github.com/wlrfx/swayfx/blob/0.6/sway/desktop/transaction.c#L730)

## What the notes found

- A 2026-10-22 source comparison separated effects from animation lifecycle. Closing animations defer destruction, and moving effects depend on arranged geometry. [docs] S-292e57b07af7
- The same study found overlapping scene modifications in scroll and scenefx, including incompatible node layouts and shadow interfaces. It treated the effect operation and the command grammar as different implementation units. [docs] S-292e57b07af7
- Blur's damage footprint grows with its pass count and radius. Scenefx calculates it as `2^(passes+1) × radius`; this is source behavior, not a frame-time measurement. [docs] [damage size](https://github.com/wlrfx/scenefx/blob/0.5/types/fx/blur_data.c#L25)
- Software-renderer checks in sway-next did not establish GPU blur output. Its [recorded state](sway-next.md) keeps that limit explicit. [docs] S-03db46ad9d28

## Where it touches Domicile

Domicile's [window compositing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md) and [stacking contracts](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/STACKING-PARITY.md) describe the current composition path. A control such as matched-window dimming is a semantic contract; the wlroots scene code remains donor-specific. [inferred]

The renderer findings connect to Domicile's [effects map](../06-features/effects/README.md). [docs]

## Lineage

- 2026-01-10: the public resize and movement animation change merged; its discussion records buffer, blur and shadow lifetime interactions. [docs] [Animation change](https://github.com/wlrfx/swayfx/pull/388)

- 2026-10-22: the investigation compared swayfx, scenefx and scroll while defining sway-next. [docs] S-292e57b07af7
- 2026-10-23: the recorded fork retained scroll's animation system and added effect command compatibility. [docs] [sway-next record](sway-next.md)

## Sources

- S-e0dd6eebeebc · 2026-08-05 · [Swayfx effect command contracts in the 0.6 release](https://github.com/wlrfx/swayfx/blob/0.6/sway/sway.5.scd#L705).
- S-ab41375a77ca · 2026-07-08 · [SceneFX blur damage calculation in the 0.5 release](https://github.com/wlrfx/scenefx/blob/0.5/types/fx/blur_data.c).
- S-292e57b07af7 · 2026-10-22 · Swayfx and scenefx source comparison
- S-03db46ad9d28 · 2026-10-23 · Renderer port and compatibility boundaries

- S-de92af3f5a9b · 2026-08-05 · [Swayfx animation and close-buffer lifetime in the 0.6 release](https://github.com/wlrfx/swayfx/blob/0.6/sway/desktop/transaction.c#L94).
- S-c96f22466372 · 2026-08-05 · [Swayfx native-client minimize handling in the 0.6 release](https://github.com/wlrfx/swayfx/blob/0.6/sway/desktop/xdg_shell.c#L395).

- S-47f30e0b6caa · 2026-08-05 · [Swayfx 0.6 release publication and base versions](https://github.com/wlrfx/swayfx/releases/tag/0.6).

- S-73cd44ceeaac · 2026-08-05 · [Swayfx foreign-toplevel minimize handling in the 0.6 release](https://github.com/wlrfx/swayfx/blob/0.6/sway/tree/view.c).
- S-d5a816264f96 · 2026-10-22 · [Preserved movement-animation discussion](https://github.com/wlrfx/swayfx/pull/388).
- S-f8f2f5e99161 · 2026-07-08 · [SceneFX 0.5 release publication](https://github.com/wlrfx/scenefx/releases/tag/0.5).
