---
id: keyboard-and-text/shortcuts-inhibit
title: Shortcut inhibition for remote desktops and games
kind: feature
family: keyboard-and-text
layers: [engine:browser, compositor, protocol, sdk]
domicile_status: landed-with-gap
domicile_refs: [cprussin/domicile#1343]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, smithay]
rows: [keyboard-shortcuts-inhibit-v1]
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:keyboard-and-text, status:landed-with-gap, evidence:source, scroll-parity:baseline]
sources: [S-016ff15271ae, S-3f524db52413, S-5ac26707cdda, S-633706e78879, S-695f43c6be84, S-7194d751e6c4, S-a90d2490bf23, S-ad6469df2811, S-cc91376be5e5, S-dfd54405f57b, S-fdd018b0e14c]
---
# Shortcut inhibition for remote desktops and games

A focused client can inhibit compositor shortcuts so its remote desktop or game receives the key combinations. Gesture binding suppression follows that state, with a native-delivery limit at the pin. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L144) [docs]

## Behavior

- The keyboard-shortcuts-inhibit manager is advertised as version 1. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1280) [source]
- Suppressing a shell gesture binding does not yet route an already grabbed sequence to the inhibiting native window. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L144) [docs]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Shortcut inhibitors interact with binding inhibition flags | A deliberately permitted escape binding can deactivate an inhibitor | [pinned seat policy](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-input.5.scd) |
| scroll | `shortcuts_inhibitor enable`, disable, activate, deactivate and toggle | Activate/deactivate/toggle apply to the focused window at runtime | [pinned manual](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-input.5.scd#L302) |

## What the notes found

- Inhibitor support and the client actually requesting an inhibitor are separate observations. A window that leaves its inhibitor state false does not test suppression. [docs]
- 2026-11-05: Scroll provides seat shortcuts_inhibitor enable|disable|activate|deactivate|toggle, with the last three runtime-only on the focused window, plus seat pointer_constraint enable|disable|escape (scroll@b6586d2) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-input.5.scd#L297> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-input.5.scd#L302>
- 2026-11-06: Domicile exposes per-window desktopId and inhibitsShortcuts; ordinary Chrome held no inhibitor, and a client taking an inhibitor on purpose blocked a shell chord while the other chords still reached the shell [docs]
- 2026-11-07: An escape chord for inhibited shortcuts can be implemented in a shell, while disabling a client's inhibitor is a compositor-level policy rather than a shell one [inferred]

## Where it touches Domicile

- `compositor` instantiates the Smithay shortcut-inhibit manager. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1280) [source]
- `engine:browser` owns shell gesture grabs before it knows which native window lies below them. That ordering explains the documented delivery limit. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L144) [docs]
- Status at the pin: landed-with-gap. Key inhibition is present; delivery of a shell-grabbed gesture to the inhibited window is not addressed at the pin. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L144) [docs]

## Lineage

- 2026-10-15: [Native shortcut inhibition](https://github.com/cprussin/domicile/pull/1343) merges upstream. [docs]

- 2026-11-04–2026-11-05: the donor comparison records runtime inhibitor escape and seat constraint controls. S-ad6469df2811 · 2026-11-04 · Seat inhibition and pointer policy comparison [docs]
- 2026-11-09: the pinned gesture contract records the inhibitor forwarding limit. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L144) [docs]

## Sources

- S-633706e78879 · 2026-11-09 · Sway seat input configuration contract.

- S-016ff15271ae · 2026-10-15 · Native shortcut inhibition merge record.

- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-a90d2490bf23 · 2026-11-09 · Native gestures, page defaults and shell binding contract.
- S-ad6469df2811 · 2026-11-04 · Seat inhibition and pointer policy comparison.
- S-dfd54405f57b · 2026-11-05 · Binding, inhibition and layout source comparison.
