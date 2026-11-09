---
id: outputs/output-management-and-power
title: Output profiles and display power
kind: feature
family: outputs
layers: [engine:drm, compositor, config, session]
domicile_status: landed-with-gap
domicile_refs: [cprussin/domicile#1548, cprussin/domicile#409, cprussin/domicile#414]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, niri]
rows: [wl_output, wlr-output-management-v1, wlr-output-power-management-v1, xdg-output-v1]
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:outputs, status:landed-with-gap, evidence:source, scroll-parity:baseline]
sources: [S-0f33ebc98d72, S-2c05e7f10be8, S-32784ffd9a8d, S-33f078435895, S-3d1446ae145e, S-3f17426ddbf7, S-4a2b900f6a8d, S-4b80a41fba84, S-5321a1bc569e, S-5a321c8ce8ea, S-6def390f53a3, S-704b41b30c0d, S-74f557738f56, S-7b4503a0d3e1, S-8100b35ed50a, S-9175c1754b40, S-920cb13b6fc5, S-93f31cb66e29, S-951964426c98, S-a70560fdc087, S-d411321b415b, S-d75a4900ec86, S-e5e4d7fe8e6a, S-ef77e5cd4455, S-f01e1a8c7909]
---
# Output profiles and display power

Display profiles determine which outputs participate, where they appear, their scale and rotation. The tty engine owns modesetting and connector power. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L6) [docs]

## Behavior

- A profile matches only when its display entries resolve to the complete connected set without reusing a monitor. The first matching profile wins. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/profile.rs#L42) [source]
- Hotplug and configuration changes re-evaluate placement. Existing outputs are rearranged by advertised name instead of being recreated. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L15) [docs]
- A profile mode checks a reported pixel size; it does not select a mode or refresh rate. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L58) [docs]
- Blanking sends an explicit disabled entry for every connector. An empty layout instead restores the hardware-driven row. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L79) [docs]

- Wayland output objects initially advertise unknown subpixel order. Restating an existing output updates its mode, transform, fractional scale and position. This reports the wire metadata, without qualifying physical panel behavior. [source] [Output construction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/outputs.rs#L53) · [Output restatement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/outputs.rs#L83)
- With a nonempty DRM layout, disabled or unnamed physical connectors receive no mode. Enabled connectors use their snapshot’s native mode; virtual network snapshots are skipped. [source] [Modeset request](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/ozone/platform/drm/domicile/drm_modeset.cc#L94)

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Output commands and management protocols configure outputs | Modes, scale filters, subpixel settings and adaptive sync are distinct controls | [pinned output manual](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-output.5.scd#L43) |
| scroll | Retains sway output configuration and timing controls | Rendering deadline and tearing options are separate from layout | [pinned manual](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L670) |

## What the notes found

- The source distinguishes the desktop logical geometry from the engine scanout row. Cursor crossing follows desktop adjacency even though connector rendering does not span between engine windows. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L103) [docs]
- 2026-11-09: Sway and Scroll choose nearest filtering at integer output scales and linear filtering at fractional scales in smart/default mode. Their scene path always uses bilinear filtering when downscaling a buffer (sway/scroll pinned references) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L650> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L744> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L193> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L209>
- 2026-11-09: Sway and Scroll request adaptive sync only on outputs reporting support. Configuration search tries requested enablement first, then retries with adaptive sync disabled when that state fails (sway/scroll pinned references) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L536> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L834> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L630> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L941>
- 2026-11-09: Sway and Scroll attempt tearing page flips only for a fullscreen view passing their view predicate with output tearing allowed. If the output-state test fails, the same frame is submitted without tearing (sway/scroll pinned references) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L253> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L296> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L269> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L314>
- 2026-11-09: Sway and Scroll delay output rendering until predicted refresh minus the output render budget. Surface callback timing also subtracts the view budget; absent budgets or insufficient remaining time cause immediate work (sway/scroll pinned references) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L177> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L320> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L193> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L344>
- 2026-11-05: sway-output documents modeline, scale_filter (linear, nearest, smart), subpixel and adaptive_sync options; no corresponding profile fields appear in Domicile's displays doc as of the pin (sway@1652c54) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-output.5.scd#L43> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-output.5.scd#L81> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-output.5.scd#L89> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-output.5.scd#L157>
- 2026-11-05: Domicile display docs describe the layout path Layout::scanout, Screens::scanout, domicile_displays_configure, engine FrameSinkBroker, OzonePlatform and DrmModeset::SetLayout; hotplug re-matching is by wl_output name so clients keep their outputs [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L74> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L18>
- 2026-11-05: Display profiles without a position are placed in one row in profile order; all connectors sit in one row on the engine desktop. Position is documented as working as in sway, within that row [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L51-L54> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L100-L111>
- 2026-11-05: Profile selection follows kanshi's model, where a profile whose displays are all plugged in wins; a profile display matches by wl_output name or by the make-model-serial description that kanshi and sway use [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L31> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L216-L226>
- 2026-11-09: DisplayLayoutStore registers a layout against a set of connected display identifiers, a structural analog of display-combination profiles (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/display/manager/display_layout_store.h#L42>
- 2026-11-09: The browser broker ignores empty display lists and replays its last nonempty list to new display observers. This is broker state propagation, separate from Wayland output advertisement (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/frame_sink_broker.cc#L326>
- 2026-11-09: The pinned display conversion conditionally selects BGRA8888 output formats when the connector snapshot lacks XBGR8888. This source fact does not establish bit-depth controls or HDR qualification [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/ozone/platform/drm/domicile/drm_screen.cc#L82>
- 2026-11-09: The SDK’s published display contract lists four rotation names and documents normal fallback for other names. That does not establish reflected-transform readback or classify intrinsic engine capability [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L367>

## Where it touches Domicile

- `config` validates matching and layout; `compositor` turns the result into an engine display request. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/profile.rs#L329) [source]
- `engine:drm` applies connector power and draws each output at its scale and transform. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L68) [docs]
- Status at the pin: landed-with-gap. Output profiles and blanking exist; mode and refresh-rate selection remain outside the profile contract. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md#L35) [docs]

- New browser display observers receive the last nonempty display list. The broker ignores empty reports; this retained list is separate from Wayland output advertisement and connector modesetting. [source] [Display report retention](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/frame_sink_broker.cc#L326)

## Lineage

- 2026-10-25: [Connector power on idle blanking](https://github.com/cprussin/domicile/pull/1548) merges upstream. [docs]

- 2026-09-18: [Output profile placement](https://github.com/cprussin/domicile/pull/414) merges upstream. [docs]

- 2026-09-17: [Output profile configuration](https://github.com/cprussin/domicile/pull/409) merges upstream. [docs]

- 2026-11-04–2026-11-05: the output study distinguishes profile mode assertions from native display-mode selection. S-d75a4900ec86 · 2026-11-05 · Output profile and engine display study [docs]
- 2026-11-09: pinned source matches the complete connected output set and applies layout independently of mode choice. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/profile.rs#L42) [source]

## Sources

- S-ef77e5cd4455 · 2026-11-09 · Browser display report retention and observer replay.

- S-8100b35ed50a · 2026-11-09 · Pinned upstream behavior and client limits.

- S-7b4503a0d3e1 · 2026-11-09 · Sway output configuration contract.

- S-4a2b900f6a8d · 2026-11-09 · Scroll command and configuration contract.

- S-d411321b415b · 2026-11-09 · Physical connector mode request construction.

- S-9175c1754b40 · 2026-11-09 · Wayland output metadata and restatement.

- S-0f33ebc98d72 · 2026-10-25 · Connector power on idle blanking merge record.

- S-6def390f53a3 · 2026-09-18 · Output profile placement merge record.

- S-4b80a41fba84 · 2026-09-17 · Output profile configuration merge record.

- S-32784ffd9a8d · 2026-11-09 · Idle activity, blanking and inhibitor contract.
- S-74f557738f56 · 2026-11-09 · Output profiles, pointer transforms and gamma contract.
- S-d75a4900ec86 · 2026-11-05 · Output profile and engine display study.
- S-e5e4d7fe8e6a · 2026-11-08 · Dated desktop behavior observations.
- S-f01e1a8c7909 · 2026-11-09 · Exact-set output profile matching and placement.
