---
id: keyboard-and-text/keybinding-model
title: Native chords and the typed key table
kind: feature
family: keyboard-and-text
layers: [engine:browser, compositor, sdk, manganese]
domicile_status: landed
domicile_refs: [cprussin/domicile#1343]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll]
rows: [wl_keyboard]
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:keyboard-and-text, status:landed, evidence:source, scroll-parity:baseline]
sources: [S-016ff15271ae, S-063bb48a06e4, S-0923bf6af493, S-0f7230f8f6a6, S-1085f9e60850, S-25350b9112e8, S-2c05e7f10be8, S-3432aac5652e, S-3f524db52413, S-4872dd803781, S-4a2b900f6a8d, S-5f93ab2e0c08, S-5ff85c78489f, S-6a02583b2e10, S-b7ec412464b4, S-bcc86c9a88ba, S-d5b15decb594, S-dfd54405f57b, S-ecfec2f17332, S-fbe8a9d8422d]
---
# Native chords and the typed key table

The shell claims native key chords and receives typed shortcut events even when a browser guest has keyboard focus. Modes, commands and default bindings remain shell policy. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/shortcut_registry.h#L55) [source]

## Behavior

- Modifier aliases resolve to one canonical spelling. Duplicate spellings of the same chord in one mode are rejected. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/own-keybindings.ts#L32) [source]
- Shortcut events contain the canonical chord, evdev keycode and resolved modifiers. Release events let the shell pair a held chord with its end. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/keyboard.ts#L9) [source]
- Keymap selection applies to the shared desktop keyboard state. Replacing the configured list compiles it before publication, preserves the selected name when present and otherwise selects the first map. The read and selection calls remain available while locked. [Selection API](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/keyboard.ts#L49) · [Configured list replacement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/config_adoption.rs#L145) [source]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Keysym and keycode bindings share modes and command dispatch | A compositor sees keys before the client | [pinned binding implementation](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/bind.c) |
| scroll | Retains sway binding semantics and adds layout commands | Input-device and inhibition options remain binding attributes | [pinned manual](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd) |

## What the notes found

- The notes distinguish the transport of a shortcut from a particular keyboard layout or personal table. The reusable part is a typed chord and its lifetime. [docs]
- 2026-11-09: Renderer consumers differ: page chords and gestures check focused-window inhibition; native ShortcutPressed fires directly. Separate document-to-native forwarding filters browser key repeats. Final seat delivery and donor per-seat override controls remain unqualified (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L987> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1639> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1645> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L2301> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1302> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1323>
- 2026-11-09: GTK 4.20.0 schedules repeat with a per-seat client timer and advertised repeat information when available. It checks keymap repeatability, stops on keyboard focus loss, and awaits display sync before emitting a timer repeat (gtk@4.20.0) [source] <https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L1131> <https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L1264> <https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L1321> <https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L1468> <https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L1606>
- 2026-11-09: Qt Wayland 6.9.0’s xkbcommon keyboard path starts a client timer for repeatable pressed keys when the advertised rate is positive. Timer firings synthesize auto-repeat release/press pairs; losing focus stops the timer (qtwayland@6.9.0) [source] <https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputdevice.cpp#L67> <https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputdevice.cpp#L1347> <https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputdevice.cpp#L1375> <https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputdevice.cpp#L1403>
- 2026-11-09: The renderer gates recognized page chords and gesture dispatch using the keyboard-focused window’s updated inhibition flag. Native ShortcutPressed calls FireShortcut directly, without a local repeat/inhibition check; upstream production and donor per-seat overrides remain separately unqualified (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L987> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1639> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1645> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L2301>
- 2026-11-09: For native keyup, the renderer passes a recorded press-owner identifier when present, otherwise the current target. Blur, pagehide and browser-guest focus trigger releases for held entries; this identifies request metadata, not final client delivery (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1118> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1341> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1369> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1384>
- 2026-11-09: Startup resolves the shell key table from the configured keyboard. Reload rebinds against the active keymap, storing the result before broadcasting it; compilation errors retain the previous keys. This table replacement is separate from native key-repeat production [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L858> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/shell_config.rs#L15> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/config_adoption.rs#L257>
- 2026-11-09: The native Wayland client creates an extended keyboard when the Chromium keyboard protocol is bound. It handles peek key events and acknowledges unhandled keys with their serial; peek support also changes duplicate release dispatch (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_keyboard.cc#L166> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_keyboard.cc#L457>
- 2026-11-09: The shared native keyboard converter maps libinput pressed/released state into EV_KEY values and delegates to ProcessEvents. This acquisition adapter alone does not settle renderer or native-app repeat filtering [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/libinput_keyboard_event_converter.cc#L84> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/libinput_keyboard_event_converter.cc#L99>
- 2026-11-09: The renderer’s document-to-native forwarding consumes keydown, then drops browser repeats before sending a pressed Key request. Page-taken presses are recorded for release suppression. This establishes a producer guard separate from repeat-rate advertisement (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1302> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1323>
- 2026-11-04: Domicile docs state the exec binding command takes an argv spawned from the compositor PATH, not a sh -c string [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L59> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L162>
- 2026-11-09: Keymap adoption compiles every configured map before publishing a replacement list or retyping the desktop. On success it retains the named active map when present, otherwise selecting the first configured map [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/config_adoption.rs#L145>
- 2026-11-09: The compositor’s keysym-table builder examines every level of the first XKB layout and chooses the lowest keycode per symbol, converted to evdev by subtracting eight. Requested chord modifiers remain independent of the symbol’s level [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/keymap.rs#L55>
- 2026-11-09: KeyboardConfig declares xkb rules, model, layout, variant, options and named keymaps, rejecting unknown fields. Repeat tuning, raw keymap files and initial lock states are not declared in that schema [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/lib.rs#L60>
- 2026-11-09: PageKeyDown consumes matched desktop chords and drops repeats before shortcut dispatch. Unmatched or inhibited chords return first. This establishes the page-shortcut guard; native-window repeat request production remains unqualified by this range (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1008>
- 2026-11-05: Manganese binds sway-named commands on Meta (Mod4) in mode default, and runManganese({ keybindings }) replaces the default map [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L3>

## Where it touches Domicile

- `engine:browser` owns the process-wide accelerator registry because a focused browser guest bypasses the shell document. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/shortcut_registry.h#L55) [source]
- Closing a page retains its claims until the reloaded page restates or releases them, avoiding a temporary key leak during reload. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/shortcut_registry.h#L96) [source]
- `sdk` canonicalizes chords and exposes keymap switching. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/own-keybindings.ts#L59) [source]
- `manganese` supplies command parsing; its exec action receives an argument vector rather than a shell expression. [key command contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L59) [docs]
- Status at the pin: landed. Binding modes and their window-policy effects are described with the shell semantics map. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/own-keybindings.ts#L18) [source]

## Lineage

- 2026-10-15: [Native chord ownership](https://github.com/cprussin/domicile/pull/1343) merges upstream. [docs]

- 2026-11-04–2026-11-05: the notes distinguish command argument vectors, mode tables and engine-owned chord routing. S-ecfec2f17332 · 2026-11-04 · Native binding command comparison [docs]
- 2026-11-09: the pinned SDK canonicalizes bindings and keeps accelerator arbitration above browser guests. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/shortcut_registry.h#L55) [source]

## Sources

- S-25350b9112e8 · 2026-11-09 · Compiled keymap-list replacement and selected-name retention.

- S-d5b15decb594 · 2026-11-09 · Manganese keyboard binding and mode contract.

- S-4a2b900f6a8d · 2026-11-09 · Scroll command and configuration contract.

- S-016ff15271ae · 2026-10-15 · Native chord ownership merge record.

- S-4872dd803781 · 2026-11-09 · SDK chord spelling and per-mode binding tables.
- S-5f93ab2e0c08 · 2026-11-09 · Browser accelerator arbitration and release delivery.
- S-6a02583b2e10 · 2026-11-09 · Browser shortcut registry lifetime contract.
- S-bcc86c9a88ba · 2026-11-09 · SDK shortcut, modifier and keymap interface.
- S-ecfec2f17332 · 2026-11-04 · Native binding command comparison.
