---
id: outputs/output-rendering-controls
title: Output rendering controls
kind: feature
family: outputs
layers: [engine:drm, compositor, protocol, config]
domicile_status: unknown
domicile_refs: []
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll]
rows: [color-management-v1, presentation-time, tearing-control-v1]
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:outputs, status:unknown, evidence:source, scroll-parity:baseline]
sources: [S-083adea7e7ad, S-33f078435895, S-3d1446ae145e, S-3f17426ddbf7, S-3f524db52413, S-515bf507459b, S-6b79c52fc5cd, S-703c0d408b29, S-9175c1754b40, S-93f31cb66e29, S-951964426c98, S-9c59f8ebc0df, S-b9df7238ba4e, S-d411321b415b]
---
# Output rendering controls

Output filtering, subpixel order, adaptive sync, render depth, HDR, tearing and render budgets are separate choices in the pinned Sway and Scroll implementations. Monitor placement, gamma ramps and client scaling have their own contracts. [source] [Sway output state](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L482) · [Scroll output state](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L630)

See [output profiles and power](output-management-and-power.md), [night light and gamma](per-output-color-and-gamma.md), and [fractional scale and viewporter](fractional-scale-and-viewporter.md).

## Behavior

- Filtering selects how a scene buffer is sampled; the downscaling path can override the output's configured filter. [source] [Sway](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L193) · [Scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L209)
- Subpixel order is output metadata. Adaptive sync and tearing are separate output-state requests with capability or acceptance checks. [source] [Subpixel state](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L507) · [Adaptive-sync search](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L834) · [Tearing test](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L296)
- A render-depth request selects candidate formats; it does not itself qualify a panel's bit depth. HDR additionally checks output color capabilities and renderer support. [source] [Format search](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L898) · [HDR checks](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L351)
- Render budgets delay rendering and surface frame callbacks relative to a predicted refresh. This code path is not a measured latency result. [source] [Output scheduling](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L320) · [Surface callbacks](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L177)

## How donors do it

The detailed conditions and paired references are in the [control rows](rendering-controls.tsv).

| Donor at pin | Mechanism | Observable behavior or limit | Links |
|---|---|---|---|
| Sway | Output-state search plus scene and frame scheduling | Capability checks, fallback formats and render budgets remain separate controls | [Configuration](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L482) · [Scene and scheduling](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L177) |
| Scroll | Retains the corresponding output-state and presentation paths | The same control categories remain separate from scrolling layout policy | [Configuration](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L630) · [Scene and scheduling](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L193) |

Sway and Scroll default each view to hint mode. An explicit per-view override permits or refuses tearing; hint mode accepts an asynchronous client presentation hint. Fullscreen, the output flag and output-state acceptance remain additional checks. [source] [Sway predicate](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/view.c#L1290) · [Scroll predicate](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1510)

## What the notes found

- A selected setting, a reported capability and an accepted output state are distinct source facts. The donor paths retain those checks rather than turning a setting into proof of physical behavior. [source] [Adaptive-sync fallback](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L834) · [Tearing fallback](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L314)
- Gamma-ramp ownership, output HDR descriptions and surface color metadata remain separate contracts; the [color and gamma feature](per-output-color-and-gamma.md) covers the ramp path. [source] [HDR and profile exclusion](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L557)
- 2026-11-05: Both donors’ view tearing predicate returns false/true for explicit per-view overrides; hint mode permits only async client presentation hints. New views start in hint mode. Fullscreen/output eligibility and retry fallback remain separate (sway@1652c54b73f67df17b7b4ab0b0f7048204aa8104;scroll@b6586d273fed7d187744b7eaec2c234386a029fa) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/view.c#L110> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/view.c#L1290> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L113> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1510>
- 2026-11-05: From a per-output render context, Scroll animates only that enabled, accepted output; without that context it visits enabled outputs. This establishes dispatch scope, with physical frame timing unverified (scroll@b6586d273fed7d187744b7eaec2c234386a029fa) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L1924>
- 2026-11-09: Empty submit damage denotes full-box repaint in the documented contract. Partial mapping requires matching prior geometry, crop and buffer size without buffer transformation; changed cases use full damage. Physical stale-pixel correctness remains unverified (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L351>

## Where it touches Domicile

- Status at the pin: unknown for this complete control set. The narrower source behaviors below do not establish a merged-PR and pinned-documentation pairing for the full feature.
- `engine:drm` builds requests using each enabled physical connector's native snapshot mode; disabled or unnamed connectors in a nonempty layout receive no mode. Mode and refresh selection remain in the [profile feature](output-management-and-power.md). [source] [Modeset construction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/ozone/platform/drm/domicile/drm_modeset.cc#L94)
- `compositor` initializes Wayland output metadata with unknown subpixel order and restates mode, transform, fractional scale and position on existing outputs. [source] [Output construction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/outputs.rs#L53) · [Restatement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/outputs.rs#L83)
- `engine:drm` conditionally requests BGRA8888 output formats when the connector snapshot lacks XBGR8888. That fallback alone supplies no HDR or deep-color qualification. [source] [Format fallback](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/ozone/platform/drm/domicile/drm_screen.cc#L82)
- `compositor` advertises presentation-time feedback. Its armed callback deadline can answer an unreported frame while discarding presentation feedback; a frame callback alone does not prove visible-pixel time. [source] [Presentation registration](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L796) · [Feedback delivery](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/frame_pacing.rs#L152) · [Deadline fallback](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/frame_pacing.rs#L85)
- The [registration audit](../../03-wayland-protocols/coverage-map.md) finds no color-management-v1 or tearing-control-v1 global across the 151 executable compositor Rust modules at this pin, with client binaries excluded. This is a client-protocol disposition; it does not establish the absence of every engine color or presentation behavior. [source] [Registration scope](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793)

## Lineage

- 2026-11-09: the fixed Sway and Scroll references retain separate filtering, metadata, synchronization, color and timing paths. [source] [Sway output configuration](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L482) · [Scroll output configuration](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L630)
- 2026-11-09: Domicile's pinned mode, metadata and feedback facts are recorded separately from the unknown status of the complete rendering-control set. [source] [Mode requests](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/ozone/platform/drm/domicile/drm_modeset.cc#L94) · [Output metadata](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/outputs.rs#L53)

## Sources

- S-6b79c52fc5cd · 2026-11-05 · Scroll per-view tearing override and client-hint predicate.

- S-515bf507459b · 2026-11-05 · Sway per-view tearing override and client-hint predicate.

- S-93f31cb66e29 · 2026-11-09 · Sway output configuration and fallback search at the fixed pin.
- S-951964426c98 · 2026-11-09 · Scroll output configuration and fallback search at the fixed pin.
- S-3f17426ddbf7 · 2026-11-09 · Sway scene filtering, tearing tests and render scheduling.
- S-3d1446ae145e · 2026-11-09 · Scroll scene filtering, tearing tests and render scheduling.
- S-d411321b415b · 2026-11-09 · Domicile physical connector modeset request construction.
- S-9175c1754b40 · 2026-11-09 · Domicile output metadata construction and restatement.
- S-33f078435895 · 2026-11-09 · Domicile conditional output-format fallback.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and source audit.
- S-083adea7e7ad · 2026-11-09 · Presentation feedback and timer fallback.
- S-b9df7238ba4e · 2026-11-09 · Frame callback lifetime and deadline.
