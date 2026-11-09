---
id: bindings-and-modes/binding-modes
title: Keybinding modes and chord ownership
kind: feature
family: bindings-and-modes
layers: [sdk, engine:browser, compositor, manganese]
domicile_status: landed
domicile_refs: [cprussin/domicile#1913, docs/architecture/KEYBINDINGS.md, packages/chrome-sdk/src/bind-keys.ts]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, jay]
eras: []
rows: [SW-0003, SW-0004, SW-0010, SW-0022, SW-0023, SW-0027, SW-0177, SW-0188, SW-0189, SW-0190, SW-0191, SW-0224, SW-0225, SW-0236, SW-0237, SW-0238, SW-0239, SW-0289, SW-0290, SC-0001, SC-0090, SC-0091, SC-0109, SC-0203, SW-0349, SW-0350, SW-0353, SW-0354, SW-0355, SW-0356, SW-0359, SW-0360, SW-0552, SW-0553, SW-0554, SW-0555, SW-0556, SW-0557, SW-0558, SW-0559, SW-0560, SW-0561, SW-0562, SW-0563, SW-0564, SW-0565, SW-0566, SW-0567, SW-0568, SW-0569, SW-0570, SW-0571, SW-0572, SW-0573, SW-0574, SW-0575, SW-0576, SW-0624, SW-0634, SW-0635, SW-0636, SW-0637, SW-0638, SW-0639, SW-0640, SW-0641, SW-0642, SW-0643, SW-0644, SW-0645, SW-0646, SW-0647, SW-0648, SC-0209, SC-0210, SC-0211, SC-0212, SC-0213, SC-0338, SC-0339, SC-0365, SC-0366, SC-0538, SW-1047, SW-1048, SW-1049, SW-1050, SW-1051, SW-1053, SW-1054, SW-1055, SW-1057, SW-1119, SW-1120, SW-1143, SC-0618, SW-1249, SW-1250, SW-1251, SW-1252, SW-1253, SW-1254]
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: false
bucket: domicile
tags: [family:bindings-and-modes, layer:sdk, status:landed, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-1b032775d560, S-204a969601d0, S-28be5e927839, S-3606bb231e4a, S-537297eb1290, S-625c4e474be6, S-77375ab2e5f5, S-7f2d34d0bfe7, S-8c438f028a0f, S-9c59f8ebc0df, S-9fe5a8a3f191, S-c40e302fa0da, S-d2242e1a7d7f, S-d5b15decb594, S-e33fe2e31922, S-e4ff18df1f16, S-fbe8a9d8422d, S-fd0c87388c02]
---
# Keybinding modes and chord ownership

A binding mode changes which action a grabbed chord or gesture runs, while the engine retains ownership of matching the physical input.

## Behavior

- Chords use named modifiers and keysyms; modifiers match exactly.
- Switching mode changes dispatch while every chord used by any mode remains grabbed.
- Unbinding releases each acquired chord and gesture.
- A client’s shortcuts inhibitor can receive grabbed keys while focused.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| sway @ pinned source | Mode blocks and binding flags | Release, repeat, input-device matching, locked and inhibited behavior are independent binding options. | [Binding grammar](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L435) · [Modes](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L834) |
| scroll @ pinned source | Inherited binding modes plus layout modes | Binding modes and the separate per-workspace insertion modes are different mechanisms. | [Layout modes](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L926) |
| Jay @ v1.15.0 | Stacked, inherited and latched binding modes | A latched mode ends after a shortcut; inherited shortcuts can be explicitly removed. [docs] | [Release reference](https://github.com/mahkoh/jay/blob/v1.15.0/book/src/input-modes.md#L3) · [Release reference](https://github.com/mahkoh/jay/blob/v1.15.0/book/src/input-modes.md#L22) · [Release reference](https://github.com/mahkoh/jay/blob/v1.15.0/book/src/input-modes.md#L55) |

## What the notes found

- Checked 2026-11-09: The SDK bindKeys helper keeps mode state in its binding instance. Manganese stores one active mode in the desk-shared window state. [source] [SDK helper](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/bind-keys.ts#L81) · [Desk state](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L189)
- Checked 2026-11-09: Scroll’s per-binding animation suppression applies to the dirty transaction committed for that binding; later jump selection or script commands can form separate transactions. [source] [Binding flag](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/bind.c#L654) · [Transaction scope](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L2119)
- Checked 2026-11-09: SDK completion matching favors explicit finger-count or edge selectors over wildcards, then the binding with more requested directions. Swipe completion adds a diagonal direction when minor-axis travel reaches half the dominant-axis travel. [source] [SDK completion rule](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L203) · [SDK completion rule](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L225)
- Checked 2026-11-09: The SDK tracker completes hold bindings only on an uncanceled end. Its completion result carries no duration threshold; begin-time ownership and any backend hold recognition remain separate. [source] [SDK completion rule](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L143) · [SDK completion rule](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L167) · [SDK completion rule](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L190)
- Checked 2026-11-09: The renderer checks the keyboard-focused window’s inhibition flag for recognized page chords and gesture dispatch. Its native ShortcutPressed callback enters FireShortcut directly; the document-to-native key producer has a separate repeat guard. [source] [Page check](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L987) · [Native and gesture callbacks](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1639)
- Checked 2026-11-09: The document-to-native key producer consumes browser repeat keydown events before sending a pressed Key request. Page-taken presses are recorded to suppress their paired release. The compositor’s advertised client repeat rate is a separate contract. [source] [Native keydown producer](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1302) · [Repeat guard](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1323)
- 2026-11-09: The pinned [default binding table](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L15-L21) lists the stock chords and the commands left unbound by default. [source]
- 2026-06-11: The niri audit separates repeat, cooldown and lock-time eligibility in a binding table from the event-dispatch machinery beneath it (Chromium historical studies) [docs]
- 2026-11-04: A comparison of sway commands with Manganese found that resize grow left and up shrink the focused window, the inverse of the sway wording, and that `workspace number N` has no Manganese command. [inferred]
- 2026-11-05: Stock keybindings have no commands for relative workspace moves, follow-moves, focus-output, move-workspace-to-output, mouse-wheel chords or exact-percent resize as of the pin, and several default chords differ from sway-style assignments. [docs]
- 2026-11-05: A 2026-11-05 table grouped sway binding commands by Domicile route (native helper, partial, or no stock command), covering launcher, workspaces, layout, media and screenshot groups [docs]

## Where it touches Domicile

- `compositor`: The keyboard layout supplies the keysym-to-key table. [source] [Key-table owner](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/KEYBINDINGS.md#L38)
- `engine:browser`: The browser process resolves grabbed chords against that table and matches their presses. [source] [Chord resolution](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/KEYBINDINGS.md#L38) · [Browser matching](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/KEYBINDINGS.md#L79)
- `sdk`: the bindKeys helper owns instance-local mode, dispatch and cleanup. [source] [Helper state](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/bind-keys.ts#L81)
- `manganese`: ModeSet replaces the desk-shared active mode. [source] [Mode transition](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1464)
- Status: `landed`; gesture bindings share the mode tables in merged [#1913](https://github.com/cprussin/domicile/pull/1913). [source]

## Lineage

- 2026-11-08: touchpad gestures joined the shell’s keybinding and mode tables. [source] [#1913](https://github.com/cprussin/domicile/pull/1913)

## Sources

- S-9fe5a8a3f191 · 2026-09-21 · [Container commands and compositor configuration](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L1).
- S-204a969601d0 · 2026-11-06 · sway and Scroll semantic inventory.
- S-3606bb231e4a · 2026-11-09 · [Chord reservation, mode dispatch and cleanup](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/bind-keys.ts#L1).
- S-537297eb1290 · 2026-11-09 · [Key tables, grabs and shell-command routing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/KEYBINDINGS.md#L1).
- S-e4ff18df1f16 · 2026-11-09 · [Desk-wide page and display coordinate ownership](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md#L1).
- S-d5b15decb594 · 2026-11-09 · [Window command vocabulary and default bindings](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L1).
- S-fd0c87388c02 · 2026-11-08 · Public Domicile change catalog.
- S-7f2d34d0bfe7 · 2026-11-05 · [Binding execution and animation transaction flags](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/bind.c#L1).
- S-9c59f8ebc0df · 2026-11-05 · [Scene arrangement and animation transaction control](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L1).
- S-77375ab2e5f5 · 2026-09-28 · Jay release documentation.
- S-8c438f028a0f · 2026-11-09 · [Desk-wide window reducer and workspace policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1).
- S-1b032775d560 · 2026-11-09 · [Shell modes and modal command guards](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/useKeybindings.ts#L1).
- S-d2242e1a7d7f · 2026-11-09 · [Gesture completion and selector specificity](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L1).
- S-fbe8a9d8422d · 2026-11-09 · [Native focus, key forwarding and desktop events](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1).
