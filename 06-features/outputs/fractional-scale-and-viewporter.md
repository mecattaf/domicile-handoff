---
id: outputs/fractional-scale-and-viewporter
title: Fractional scaling and client viewports
kind: feature
family: outputs
layers: [engine:drm, engine:blink, compositor, protocol, config]
domicile_status: landed
domicile_refs: [cprussin/domicile#933]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, smithay]
rows: [fractional-scale-v1, viewporter]
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:outputs, status:landed, evidence:source, scroll-parity:baseline]
sources: [S-2c05e7f10be8, S-3f524db52413, S-43af2b3af183, S-46587779628d, S-578137f975a9, S-74f557738f56, S-9175c1754b40, S-920cb13b6fc5, S-e7392f226f69, S-f01e1a8c7909]
---
# Fractional scaling and client viewports

Fractional output scale separates physical buffer pixels from desktop logical coordinates. Viewports let a client express which buffer area maps to its surface size. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L56) [docs]

## Behavior

- Profiles accept a positive fractional scale and an output rotation. Logical dimensions derive from the rotated mode divided by that scale. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/profile.rs#L134) [source]
- Core output scale remains an integer rounded upward. The xdg output carries logical dimensions, and the fractional-scale object receives its preferred scale. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/outputs.rs#L95) [source]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Fractional output scale with viewporter-aware clients | A client chooses its own buffer resolution and viewport | [pinned server](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/server.c#L540) |
| scroll | Retains sway fractional-scale and viewporter interfaces | Workspace content scaling is an additional layout semantic | [pinned server](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/server.c#L543) |

## What the notes found

- The scale of an output, client buffer transform, viewport destination and shell element transform are different coordinate conversions. A screenshot alone does not identify which one supplied a change. [inferred]
- 2026-11-09: The complete output type declares described displays, maximum nested scale and monitor profiles. Described displays advertise nonzero integer scale; physical profiles document fractional scale. These layout contracts do not establish donor sampling, subpixel or HDR behavior (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/lib.rs#L264> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/lib.rs#L376> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L56>
- 2026-11-09: ManagedDisplayInfo stores zoom values per display size and combines zoom with device scale; this is distinct from proving a particular fractional-scale wire implementation (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/display/manager/managed_display_info.h#L194> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/display/manager/managed_display_info.h#L256>
- 2026-11-05: Domicile display profiles carry a fractional scale value and a rotate-N transform name; rotate-270 matches wl_output and kanshi conventions, and the rotated monitor's cursor edges are read in the rotated frame [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L41> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L194-L195>

## Where it touches Domicile

- `compositor` instantiates viewporter and fractional-scale managers. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) [source]
- `compositor` reports `Scale::Fractional` and sends preferred scale when the object is created. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/outputs.rs#L95) [source]
- `engine:drm` renders the per-monitor logical page upright at the configured output transform. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L174) [docs]
- Status at the pin: landed. Output scale is part of the pinned profile and Wayland reporting paths. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/profile.rs#L134) [source]

## Lineage

- 2026-10-05: [Native-window preferred output scale](https://github.com/cprussin/domicile/pull/933) merges upstream. [docs]

- 2026-11-09: pinned source keeps fractional logical geometry and integer core output scale separate. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/outputs.rs#L95) [source]

## Sources

- S-46587779628d · 2026-10-05 · Native-window preferred output scale merge record.

- S-43af2b3af183 · 2026-11-09 · Pinned donor implementation locator.

- S-e7392f226f69 · 2026-11-09 · Pinned donor implementation locator.

- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-74f557738f56 · 2026-11-09 · Output profiles, pointer transforms and gamma contract.
- S-9175c1754b40 · 2026-11-09 · Wayland output creation and fractional-scale reporting.
- S-f01e1a8c7909 · 2026-11-09 · Exact-set output profile matching and placement.
