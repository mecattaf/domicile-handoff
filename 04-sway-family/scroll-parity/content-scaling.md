---
id: sway-family/scroll-parity/content-scaling
title: Content scaling
kind: family
family: windows
layers: [engine:blink, compositor, sdk]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-1fa1acbff487, S-204a969601d0, S-23c521755f22, S-4a2b900f6a8d, S-6b79c52fc5cd, S-9c59f8ebc0df, S-a70560fdc087, S-d5b15decb594, S-e879a459bb23]
---

# Content scaling

13 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 0 same; 1 scroll-changed; 12 scroll-only; 0 sway-only.
- Domicile coverage: 0 landed; 0 partial; 10 absent; 1 unverified; 2 accounting aliases/exclusions.
- Mechanism layers: compositor 13; engine:blink 13; sdk 1.
- Feature: [windows/content-scaling](../../06-features/windows/content-scaling.md).

## Donor semantics

`scale_content` changes one window independently of output density. It accepts direct, incremental and reset operations. Xwayland has a lower bound of one because its shared coordinate canvas constrains input routing; native Wayland clients do not use that X11 guard. [source] [Content scale](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L827).

Visual scaling and client buffer density are separate parts of the contract. On transaction commit, entered outputs contribute their density multiplied by the window's content factor; the maximum starts at one. With no entered output, the implementation instead uses the workspace output as a fallback. It notifies fractional scale and sends the ceiling as preferred integer scale. [source] [Density negotiation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L2219).

The SDK forwards CSS window bounds; the compositor chooses native density from the display with the largest overlap. The native window record has no independent content-density control. Browser pinch zoom and ordinary CSS transforms do not by themselves establish this donor negotiation contract. [source] [Bounds](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/windows.ts#L38) · [Density selection](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screens.rs#L752) · [Window fields](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1139).

Initial density notification precedes the later scene-scale negotiation: after selecting the initial workspace/output, Scroll sends that output's fractional scale and its rounded-up integer buffer scale. This early mapping call does not multiply in per-view content scale. [source] [Initial notification](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L938).

Scroll’s scene computes preferred fractional density as the maximum of one and every entered output scale multiplied by the view content factor; integer buffer scale is its ceiling. Presentation feedback and callbacks separately choose the highest-refresh nonsuspended current output. Density choice and frame-pacing output choice therefore remain separate contracts. [source] [Density and pacing selection](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/types/scene/surface.c#L25) · [Density and pacing selection](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/types/scene/surface.c#L156) · [Density and pacing selection](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/types/scene/surface.c#L39) · [Density and pacing selection](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/types/scene/surface.c#L182).

The donor content-scale command requires a leaf window and connected output and rejects ordinary/layout fullscreen. Explicit content factors clamp to 0.2–4.0, with an additional floor of one for Xwayland. Reset clears the explicit stored factor; output-density compensation remains separate. [source] [Command guards](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/scale.c#L12) · [Factor bounds](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1543) · [Reset](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1643).

## At the pin

Not addressed at the pin: the complete native window record has no per-window content-density factor; setBounds selects monitor density. Browser-page pinch zoom and CSS visual transforms are distinct. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1139) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/windows.ts#L38) · [Related change](https://github.com/cprussin/domicile/pull/1944).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-1fa1acbff487 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-6b79c52fc5cd · 2026-11-05 · Scroll reference at the donor revision.
- S-9c59f8ebc0df · 2026-11-05 · Scroll reference at the donor revision.
- S-a70560fdc087 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-e879a459bb23 · 2026-11-05 · Scroll reference at the donor revision.
