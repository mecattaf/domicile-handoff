---
id: pointer/cursor-shapes-and-constraints
title: Cursor shape, motion and constraints
kind: feature
family: pointer
layers: [engine:aura, engine:drm, compositor, protocol, config]
domicile_status: landed-with-gap
domicile_refs: [cprussin/domicile#1864]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, smithay, hyprland]
rows: [cursor-shape-v1, pointer-constraints-v1, relative-pointer-v1, wl_pointer]
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:pointer, status:landed-with-gap, evidence:source, scroll-parity:baseline]
sources: [S-22ebd31e2365, S-24f0ff5009b7, S-2d2a0af0df57, S-32a5e1e4befb, S-33889728083c, S-3f524db52413, S-472d3a68890f, S-5b1bd336dbff, S-74f557738f56, S-7ca0634a8080, S-7fd9d5a1a94c, S-8100b35ed50a, S-a89a4c46b8f9, S-a92c840fc324, S-e218e6d49ccf, S-e7392f226f69, S-ecfec2f17332, S-efda772dfa01, S-fd0c87388c02]
---
# Cursor shape, motion and constraints

Named cursor shapes are served through the cursor-shape protocol; the tty engine selects and draws the image at the active display scale and rotation. Pointer locking and relative motion are separate protocols. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1302) [source]

## Behavior

- Theme images retain their hotspot and animation frames when scaled and turned. Captured cursor images are transformed upright for the capture source. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/ozone/platform/drm/domicile/drm_cursor_factory.cc#L64) [source]
- A missing theme image falls back to Chromium cursor assets. A cursor type denoting no image remains hidden. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/ozone/platform/drm/domicile/drm_cursor_factory.cc#L68) [source]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Relative motion is delivered before cursor clipping | Both accelerated and unaccelerated deltas are sent; locking uses an empty movement region | [Motion route](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/cursor.c#L290) · [Constraint region](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/cursor.c#L842) |
| scroll | Runtime constraint escape applies to a named seat or all existing seats | It requests deactivation while permitting future constraints; configuration-file escape is rejected | [Escape command](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/seat/pointer_constraint.c#L36) · [Deactivation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/cursor.c#L1386) |
| Hyprland @ source pin | Held-button focus is retained outside drag-and-drop | Removing a pointer synthesizes release for its own held buttons | [Pointer routing](https://github.com/hyprwm/Hyprland/blob/ab6141897d57e609e24c571a762c30c963e0885a/src/managers/input/InputManager.cpp#L391) · [Device removal](https://github.com/hyprwm/Hyprland/blob/ab6141897d57e609e24c571a762c30c963e0885a/src/managers/input/InputManager.cpp#L1568) |

Sway and Scroll accept cursor surfaces and named shapes only from the client owning the seat’s pointer-focused surface, when the active seat operation permits a change. [source] [Sway surface](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/cursor.c#L867) · [Sway named shape](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/cursor.c#L1394) · [Scroll surface](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/cursor.c#L895) · [Scroll named shape](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/cursor.c#L1426)

## What the notes found

- The useful boundary is image choice versus motion semantics. Named shapes do not imply raw relative deltas, confinement or a locked pointer. [inferred]
- 2026-11-09: The native app content layer is hit-testable for the page. Domicile routes input to native clients separately rather than delegating that routing to Viz [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/layout_app_surface.cc#L51>
- 2026-11-09: A native-app element forwards pointer position and button state before taking DOM pointer capture on a button press. Its pointer-up path forwards the button release [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_app_element.cc#L495>
- 2026-11-09: At the pin a client-drawn cursor surface maps to the default arrow shape, with a source comment citing native compositing as the missing piece [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/seat.rs#L50>
- 2026-11-04: scroll documents cursor_shake_magnify with a sensitivity option: shaking the pointer magnifies the cursor (scroll@b6586d2) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L190-L194>
- 2026-11-05: The engine draws the tty cursor from theme.cursor_theme and cursor_size; cursor environment variables alone do not configure that path [docs]

## Where it touches Domicile

- `compositor` instantiates cursor-shape version 2 at the pin. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1302) [source]
- `engine:drm` loads the configured theme and transforms the selected cursor, including its hotspot and animation. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/ozone/platform/drm/domicile/drm_cursor_factory.cc#L101) [source]
- `engine:drm` also supplies upright captured cursor pixels. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/base/cursor/domicile/desk_cursor_image.cc#L47) [source]
- Status at the pin: landed-with-gap. Registration inspection covers all executable compositor modules; relative-pointer and pointer-constraints globals are absent. A client-supplied cursor surface falls back to the default arrow, as recorded in the [upstream cursor gap](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md#L16). [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/seat.rs#L42) [source]

## Lineage

- 2026-11-05: [Page-owned pointer rotation](https://github.com/cprussin/domicile/pull/1864) merges upstream. [docs]

- 2026-11-09: pinned source separates the named cursor-shape path from client-surface fallback and absent motion-constraint globals. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1302) [source]

## Sources

- S-472d3a68890f · 2026-11-09 · Seat input dispatch and client cursor fallback.

- S-32a5e1e4befb · 2026-11-09 · Runtime pointer-constraint escape command.

- S-7fd9d5a1a94c · 2026-11-09 · Scroll pointer focus, motion and constraint routing.

- S-7ca0634a8080 · 2026-11-09 · Sway pointer focus, motion and constraint routing.

- S-8100b35ed50a · 2026-11-09 · Upstream known protocol and client gaps.

- S-22ebd31e2365 · 2026-11-05 · Page-owned pointer rotation merge record.

- S-e7392f226f69 · 2026-11-09 · Pinned donor implementation locator.

- S-e218e6d49ccf · 2026-11-09 · Hyprland input configuration and device lifetime.

- S-2d2a0af0df57 · 2026-11-09 · Upright capture cursor image conversion.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-74f557738f56 · 2026-11-09 · Output profiles, pointer transforms and gamma contract.
- S-efda772dfa01 · 2026-11-09 · DRM cursor theme loading and transformed image selection.
