---
id: seats/independent-seat
title: Independent logical seats
kind: feature
family: seats
layers: [engine:input, engine:browser, compositor, protocol, any-shell, external]
domicile_status: absent
domicile_refs: []
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, niri, kitty]
rows: [ext-transient-seat-v1, wl_seat]
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:seats, status:absent, evidence:source, scroll-parity:baseline]
sources: [S-219a62420ba2, S-220a8533ce51, S-25309f3f163f, S-2871d1466900, S-2a7c4133bfd5, S-3226f0a93193, S-3533e3f0274a, S-3f524db52413, S-4aa1e405fd79, S-4ddce4f277b2, S-50ea8a1ddf1e, S-526eea98076e, S-5b97d09ecc32, S-62626c2d6829, S-64a0036312be, S-6d51bf619041, S-76824356a8c6, S-78a98efb6748, S-85696667a154, S-9939f46159cb, S-9b6eacc885db, S-9d812ac89cf4, S-9ee716b1ba9a, S-a89a4c46b8f9, S-b4058c3be3bd, S-d3166f3048c1, S-df214f6d3232, S-e4ff18df1f16, S-ed24ede20694, S-fc7fb2d036bb]
---
# Independent logical seats

A logical seat groups input devices, a pointer and keyboard focus for an independent participant. Multiple seats can support a second person, a kiosk, testing or remote input without sharing one focus. [docs]

## Behavior

- Device-access brokerage, multiple Wayland seats and a transient-seat creation protocol are distinct mechanisms. [docs]
- Separate focus does not automatically isolate visible workspaces, command context, clipboard selections, activation, shortcuts or application seat selection. S-3226f0a93193 · 2026-10-22 · Logical-seat isolation and client compatibility study [docs]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Named seats and temporary seats retain independent focus | Runtime assignment and shared workspace policy are separate concerns | [pinned input manager](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/input-manager.c#L348) |
| scroll | Retains sway seat attachment and virtual-device routing | A virtual pointer can name both a seat and an output | [pinned input manager](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/input-manager.c#L348) |
| niri | The reviewed source creates one logical seat | A virtual input path still uses that one seat | S-76824356a8c6 · 2026-10-06 · Input and logical-seat source comparison |

## What the notes found

- A bounded sway 1.12 and wlroots 0.20.2 study records independent focus for multiple remote connections. It also records runtime device reassignment and shared-workspace coupling; those are historical donor observations, not Domicile measurements. S-3226f0a93193 · 2026-10-22 · Logical-seat isolation and client compatibility study [docs]
- The same study records secondary-seat typing in foot and alacritty, while kitty 0.48.0 accepted input only from its first seat in the control. GTK/Qt behavior, drawn cursors per seat and Chrome runtime behavior remain unproven there. S-2871d1466900 · 2026-10-22 · Multi-seat client input study [docs]

- Kitty 0.48.0 binds a seat only while its stored seat is empty, confirming the first-seat constraint in pinned public client source. [Kitty seat binding](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_init.c#L604) [source]
- 2023-04-29: Any1 assigns most_recent_seat on key and pointer-frame events to the logically focused container; changing seats emits a window-focus event, repeated same-seat activity is unchanged, and seat destruction clears the association (any1@9d60831231375110a7d1b03d52a5c7df674b40bc) [source] <https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/tree/container.c#L2013> <https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/input/keyboard.c#L400> <https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/input/cursor.c#L470> <https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/ipc-json.c#L809>
- 2026-11-09: Source-selective idle activity is enforced by a per-seat mask in both donors, beyond storing configuration. Their default mask includes keyboard, pointer, touch, tablet-pad, tablet-tool and switch input (sway/scroll pinned references) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/seat.c#L106> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/seat.c#L546> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/seat.c#L108>
- 2026-11-09: The shared donor cursor-command implementation performs named-seat lookup, relative/absolute movement, button dispatch and pointer-activity reporting. A wildcard loops over existing seats; the command does not create a seat (sway/scroll pinned references) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/seat/cursor.c#L19> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/seat/cursor.c#L52> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/seat/cursor.c#L52>
- 2026-11-09: GTK 4.20.0’s Wayland IM-context module creates text-input-v3 for the display’s default seat. Its state is stored per display, and widget focus selects one current editing context (gtk@4.20.0) [source] <https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gtk/gtkimcontextwayland.c#L714> <https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gtk/gtkimcontextwayland.c#L765> <https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gtk/gtkimcontextwayland.c#L789>
- 2026-11-09: Qt Wayland 6.9.0’s input context selects the display’s default input device for its text-input interface. Editing updates use the application focus object and focus window (qtwayland@6.9.0) [source] <https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputcontext.cpp#L77> <https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputcontext.cpp#L177> <https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputcontext.cpp#L218>
- 2026-11-05: Scroll stores virtual-output maximize state per output and resolves each leaf’s active workspace from the current seat’s focus stack, falling back to a cached workspace (scroll@b6586d273fed7d187744b7eaec2c234386a029fa) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/virtual_output.c#L140> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/virtual_output.c#L150> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/virtual_output.c#L520>
- 2026-11-05: Scroll’s idle-source parser rejects the manual’s touchpad token. It accepts six named sources; deprecated idle_wake stores a distinct mask field and warns rather than calling idle_inhibit (scroll@b6586d273fed7d187744b7eaec2c234386a029fa) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/seat/idle.c#L10> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/seat/idle.c#L58> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-input.5.scd#L281>
- 2026-11-09: EIS input keeps per-client pointer position and held buttons, then translates movement and buttons into ordinary client requests. That route shares the native-client lock gate [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/translation.rs#L103> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/translation.rs#L119>
- 2026-11-09: The historical Any1 branch serializes all seats focused on a node and separately the most recently active seat for a container (any1@9d608312) [source] <https://github.com/any1/sway/blob/9d608312/sway/ipc-json.c#L764>
- 2026-11-05: sway's seat keyboard_grouping offers none (isolated state per keyboard) or smart (grouped by keymap and repeat info, the default) for keyboards that appear as several devices (sway@1652c54) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-input.5.scd#L287-L292>
- 2026-11-05: On a 2026-11-05 snapshot Domicile exposed one logical Wayland seat; additional physical mice, EIS sessions and trusted page input all fed the same pointer and keyboard focus, and the compositor dropped an app id before building a key request [docs]
- 2026-06-06: The minimal-Ash study separates per-root Aura clients from one global input-capture broker; multiple roots do not establish independent seats (Chromium historical studies) [docs]
- 2026-10-22: An October source study identifies seat-local input callbacks alongside compositor-global modal state; second-seat behavior was not runtime-qualified (scroll October study) [docs]
- 2026-11-04: An audit of an earlier main snapshot found one seat with remote-input support present and no second independent seat [docs]
- 2026-11-06: A Miracle WM multi-seat issue was closed while its maintainer still described a Mir limitation, so closure there is not evidence of delivered multi-seat support (miracle-wm) [docs]

## Where it touches Domicile

- `compositor` creates one Wayland seat, keyboard, pointer and permanent touch capability at startup. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L834) [source]
- `engine:browser` and the shell share the one desktop page described by the upstream architecture. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md#L1) [docs]
- Status at the pin: absent for independent multiple seats and `ext-transient-seat-v1`. The registration search covers the complete executable compositor module set and finds one seat construction with no transient-seat state or global. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L834) [source]

## Lineage

- 2026-10-06: the source comparison separates scroll multiple-seat semantics from niri single-seat semantics. S-76824356a8c6 · 2026-10-06 · Input and logical-seat source comparison [docs]
- 2026-10-22: a bounded donor study records focus isolation and its limits, including first-seat-only clients. S-3226f0a93193 · 2026-10-22 · Logical-seat isolation and client compatibility study [docs]

## Sources

- S-e4ff18df1f16 · 2026-11-09 · Spanning desktop page and single-seat architecture.

- S-b4058c3be3bd · 2026-11-09 · Pinned donor implementation locator.

- S-4aa1e405fd79 · 2026-11-09 · Pinned donor implementation locator.

- S-2871d1466900 · 2026-10-22 · Multi-seat client input study.
- S-3226f0a93193 · 2026-10-22 · Logical-seat isolation and client compatibility study.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-76824356a8c6 · 2026-10-06 · Input and logical-seat source comparison.
- S-9b6eacc885db · 2026-07-18 · Kitty seat and pointer-gesture registry binding.
