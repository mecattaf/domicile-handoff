---
id: sway-family/scroll-parity/seat-and-input-config
title: Seat and input configuration
kind: family
family: seats
layers: [config, engine:input, compositor, sdk, protocol, session, engine:blink, engine:drm, engine:aura, manganese, engine:browser]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-204a969601d0, S-220a8533ce51, S-23c521755f22, S-24f0ff5009b7, S-2c05e7f10be8, S-4a2b900f6a8d, S-5a321c8ce8ea, S-64a0036312be, S-85696667a154, S-d5b15decb594, S-ef437e2b7375, S-fc7fb2d036bb]
---

# Seat and input configuration

137 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 137 same; 0 scroll-changed; 0 scroll-only; 0 sway-only.
- Domicile coverage: 0 landed; 57 partial; 54 absent; 23 unverified; 3 accounting aliases/exclusions.
- Mechanism layers: compositor 62; config 107; engine:aura 2; engine:blink 7; engine:browser 9; engine:drm 1; engine:input 89; manganese 2; protocol 34; sdk 18; session 8.
- Feature: `seats/independent-seat`.

## Donor semantics

The donor configuration matches devices, assigns seats and configures keyboard layout/repeat, pointer acceleration, touchpad methods, tablet mapping, idle-source policy and shortcut inhibition. Device metadata readback, configurable settings and native event delivery are separate contracts. [source] [Input and seats](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1649).

Manganese's command guide places output/input configuration in compositor configuration rather than the page command language. This family therefore retains exact native/configuration dispositions separately from the absence of sway-compatible commands. Detailed device and seat coverage belongs to the native maps. [source] [Command boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L168).
The global keyboard configuration exposes XKB rules, model, layout, variant and options, plus complete named keymaps. Its exhaustive field declaration rejects unknown fields and has no repeat-timing, raw keymap-file or initial lock-state options. The corresponding absence rows describe public configuration; engine capability remains unverified. [source] [Keyboard fields](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/lib.rs#L60) · [Keymap scope](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-CONFIG.md#L178).
Tap-button mapping has selectable left/right/middle and left/middle/right values. Physical clickfinger instead has a documented fixed left/right/middle mapping and no separate mapping selector. Absolute tablet/touchscreen configuration identifies a device and one display; its complete type has no cropping or calibration geometry fields. [source] [Tap versus physical click](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/input_devices.rs#L140) · [Clickfinger mapping](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/input_devices.rs#L26) · [Absolute binding](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/pointers.rs#L12).

EIS input retains a pointer position and held buttons for each client, then translates them into the ordinary native-client request route with its shared lock gate. Per-client transport state does not establish an independent compositor seat or the donor named-seat cursor-command conventions. [source] [Per-client input translation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/translation.rs#L103).

The donor source selector is enforced by a per-seat activity mask, beyond storing configuration. Cursor commands look up an existing named seat; a wildcard visits existing seats and does not create one. Movement and button dispatch also report pointer activity. Domicile EIS client state and surface/application idle holds remain distinct mechanisms. [source] [Activity mask](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/seat.c#L106) · [Activity mask](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/seat.c#L546) · [Activity mask](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/seat.c#L108) · [Cursor command scope](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/seat/cursor.c#L19) · [Cursor command scope](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/seat/cursor.c#L52) · [Cursor command scope](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/seat/cursor.c#L52).

The native finger-scroll route can accelerate browser deltas from vector speed while retaining scaled linear native-axis distance. Accelerated delivery emits whole units with per-axis remainders; disabling acceleration preserves fractional linear distance. Stopping an axis clears its remainder; converter teardown emits owed stops and releases tracked buttons. Native-app wheel forwarding prefers retained axis distance, derives value120 separately and rejects synthetic momentum. These are source paths without a new device qualification. [source] [Acceleration](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/scroll_accelerator.cc#L20) · [Acceleration](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0173-domicile-configure-touchpads-and-mice-during-a-session.patch#L322) · [Axis state](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/scroll_accelerator.cc#L55) · [Axis state](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/scroll_accelerator.cc#L86) · [Native-axis retention](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0174-domicile-preserve-native-scroll-sources-and-axis-stops.patch#L472) · [Native-axis retention](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0175-domicile-share-libinput-devices-and-native-typing.patch#L868) · [Native-axis retention](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/active_scroll_axes.h#L17) · [Native forwarding](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_app_element.cc#L451).

The Scroll manual lists touchpad as an idle-source token, but the complete parser’s six-name table omits it and rejects it as invalid. Its deprecated idle_wake command stores a separate mask field and warns; that parser alone does not establish runtime use or equivalence to idle_inhibit. This mismatch is established for Scroll; Sway implementation equality remains unverified here. [source] [Documented names](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-input.5.scd#L281) · [Accepted names](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/seat/idle.c#L10) · [Legacy storage](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/seat/idle.c#L58).

## At the pin

The complete KeyboardConfig field search at the pin has no repeat-delay/rate, raw keymap-file or initial CapsLock/NumLock configuration fields and rejects unknown fields. This disposition covers exposed configuration; underlying engine capability remains unverified. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/lib.rs#L60) · [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-CONFIG.md#L178).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-220a8533ce51 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-24f0ff5009b7 · 2026-11-09 · donor source accounting and current-state evidence.
- S-2c05e7f10be8 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-5a321c8ce8ea · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-64a0036312be · 2026-11-05 · Scroll reference at the donor revision.
- S-85696667a154 · 2026-11-05 · Scroll reference at the donor revision.
- S-ef437e2b7375 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-fc7fb2d036bb · 2026-11-05 · Scroll reference at the donor revision.
