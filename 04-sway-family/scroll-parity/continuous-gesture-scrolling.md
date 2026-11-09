---
id: sway-family/scroll-parity/continuous-gesture-scrolling
title: Continuous gesture scrolling
kind: family
family: feel-and-animation
layers: [sdk, any-shell]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-0d9c2ee30def, S-1f34e9847c56, S-204a969601d0, S-23c521755f22, S-37f72d290d44, S-4a2b900f6a8d, S-8c438f028a0f, S-d5b15decb594, S-dae213c8d0b8]
---

# Continuous gesture scrolling

14 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 1 same; 0 scroll-changed; 13 scroll-only; 0 sway-only.
- Domicile coverage: 0 landed; 0 partial; 14 absent; 0 unverified; 0 accounting aliases/exclusions.
- Mechanism layers: any-shell 14; sdk 14.
- Feature: [feel-and-animation/scroll-feel-parity](../../06-features/feel-and-animation/scroll-feel-parity.md).

## Donor semantics

Continuous layout scrolling begins only when the scaled arrangement exceeds its viewport. In horizontal orientation, one gesture axis moves the whole sequence and the other moves the group beneath the pointer; vertical orientation exchanges those routes. The target under the pointer can differ from keyboard focus. [source] [Begin/update](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3340).

Each update applies configured sensitivity and workspace scale before changing positions. Ordinary animation is suppressed during direct manipulation. A pin can temporarily move out of the scrolling list so that it remains anchored. At completion, accumulated movement chooses a dominant direction and the focus routine selects an appropriate visible edge item before rearrangement. [source] [Scaling and routing](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3383) · [Completion](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3497).

Enablement, finger count, touchpad sensitivity and mouse sensitivity are independent settings. Modifier plus middle-button dragging is a second route into the same layout behavior. Discrete focus or workspace swipe bindings are not the continuous overflowing-layout contract. [source] [Configuration](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L233) · [Mouse route](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L547).

The direct-scroll target is recomputed from pointer position during updates. The pinned group is checked first, then group bands along the workspace axis, with the focused group as fallback. Begin rejects the operation when both measured extents fit; subsequent deltas are multiplied by sensitivity and current workspace scale. Updates explicitly select the disabled-animation path. Completion selects its direction from accumulated displacement, with equal axis magnitudes taking the vertical branch; it does not choose that direction from a release-velocity field. [source] [Pointer target](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3128) · [Begin and update](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3340) · [Completion](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3497).

A pin that obstructs primary-axis scrolling is temporarily moved out of the tiling sequence and restored on completion using the adjacent group's midpoint. Mouse dragging feeds cursor-position differences through the same layout updates. Its ordinary button-release path finalizes after all pressed buttons are released; loss of the referenced container follows a separate reset path. [source] [Pin suspension](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3226) · [Pin restoration](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3272) · [Mouse lifecycle](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/seatop_scroll_tiling.c#L11).

Native wheel forwarding and browser finger-scroll acceleration have separate routes and axis state. Their existence does not supply the donor’s continuous layout-scroll begin/update/end contract. [source] [Native-axis route](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0174-domicile-preserve-native-scroll-sources-and-axis-stops.patch#L472) · [Native-axis route](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0175-domicile-share-libinput-devices-and-native-typing.patch#L868) · [Native-axis route](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/active_scroll_axes.h#L17) · [Wheel forwarding](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_app_element.cc#L451).

## At the pin

Not addressed at the pin: there is no overflowing-layout scroll state/action in the complete Manganese state and command inventories; discrete focus/workspace gesture actions are separate. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L121) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35) · [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L7).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-1f34e9847c56 · 2026-11-05 · Scroll reference at the donor revision.
- S-37f72d290d44 · 2026-11-05 · Scroll reference at the donor revision.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-dae213c8d0b8 · 2026-11-05 · Scroll reference at the donor revision.
