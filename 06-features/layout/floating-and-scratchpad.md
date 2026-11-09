---
id: layout/floating-and-scratchpad
title: Floating groups and the scratchpad
kind: feature
family: layout
layers: [manganese, sdk, compositor]
domicile_status: landed
domicile_refs: [cprussin/domicile#1929, packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: true
scroll_parity: baseline
donors: [sway, scroll, cosmic, swayfx]
eras: []
rows: [SW-0133, SW-0141, SW-0163, SW-0176, SW-0204, SW-0205, SW-0206, SC-0051, SC-0082, SW-0508, SW-0509, SW-0583, SW-0584, SC-0548, SW-1070, SW-1072, SW-1077, SW-1146, SC-0694, SC-0695]
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: false
bucket: domicile
tags: [family:layout, layer:manganese, status:landed, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-23c521755f22, S-2ba44e30c201, S-4a2b900f6a8d, S-6792066180dd, S-6da8864cbb54, S-703c0d408b29, S-73cd44ceeaac, S-8c438f028a0f, S-914b44f9321b, S-947dc5b8a5d3, S-9fe5a8a3f191, S-c96f22466372, S-d5b15decb594, S-d75a4900ec86, S-e105a52826c2, S-ed569f765e51, S-fd0c87388c02]
---
# Floating groups and the scratchpad

A floating group moves independently of the tiled tree; the scratchpad hides a window and recalls it without consuming a workspace slot.

## Behavior

- Floating a group preserves its nested arrangement.
- Scratchpad recall, cycle and hide are distinct observable actions.
- A recalled window gets focus, and hiding it restores the prior workspace interaction.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| sway @ pinned source | Floating containers and scratchpad list | Repeated scratchpad show cycles hidden scratchpad windows. | [Scratchpad](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L338) |
| scroll @ pinned source | Inherited scratchpad commands within a changed layout | Hidden scratchpad windows remain separate from row and column allocation. | [Scratchpad command](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L873) |
| COSMIC @ epoch-1.11.0 | Typed minimized-window restoration state | Fullscreen, floating and tiled windows retain different restoration data. [docs] | [Release reference](https://github.com/pop-os/cosmic-comp/blob/epoch-1.11.0/src/shell/workspace.rs#L124) |
| COSMIC @ epoch-1.11.0 | Output-relative floating restoration | The floating positioning helper rescales saved geometry proportionally when the recorded output size changes. [docs] | [Release reference](https://github.com/pop-os/cosmic-comp/blob/epoch-1.11.0/src/shell/workspace.rs#L303) |
| Swayfx @ 0.6 | Optional minimize-to-scratchpad policy | With scratchpad_minimize enabled, xdg minimize adds/hides a scratchpad item and foreign-toplevel unminimize shows it. [docs] | [Minimize](https://github.com/wlrfx/swayfx/blob/0.6/sway/desktop/xdg_shell.c#L395) · [Unminimize](https://github.com/wlrfx/swayfx/blob/0.6/sway/tree/view.c#L801) |

## What the notes found

- Checked 2026-11-09: Manganese’s recalled scratchpad is modal. Its default box follows the screen margins, subject to a single native client’s min/max limits. [source] [Scratchpad behavior](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L187) · [Native float clamp](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2574)
- Checked 2026-11-09: Scroll’s taskbar unminimize path returns a window to tiling, including a window that had been floating. This is distinct from recalling a scratchpad window. [source] [Unminimize handling](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L885)
- Checked 2026-11-09: Floating movement chooses a screen from the floating box’s center and rebases its coordinates into that screen’s shown workspace. A center outside every screen keeps the prior screen; a float on an unshown workspace does not move. [source] [Floating movement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2270) · [Screen membership](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2410)
- 2026-11-09: Ash picture-in-picture entry disables ordinary widget activation, adds the widget to a focus cycler and excludes the window from MRU tracking. Leaving picture-in-picture reverses those registrations (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L1246>
- 2026-11-05: A Manganese window-management source file defines floating rules as strict objects matching an app pattern or a title pattern (at least one required), with an empty default list and dialogs floating without a rule, modeled on sway for_window floating enable [docs]
- 2026-11-05: At an earlier snapshot of main, Manganese supported floating rules although the keys guide stated that every window opens tiled [docs]

## Where it touches Domicile

- `manganese`: selected groups can float with their internal tree. Scratchpad show cycles hidden windows; bring and hide support one-way interaction. [source] [Floating groups](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L161) · [Scratchpad](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L187)
- `sdk`: The shell reports a hidden native window with an empty bounds box through `windows.setBounds`. [source] [Hidden-window API](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/docs/ELEMENTS.md#L34)
- `compositor`: A native window reported hidden has frame callbacks held and receives the suspended state; a window cast exempts that window from suspension. [source] [Hidden native clients](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L200)
- Status: `landed`; the directional scratchpad gesture slice is merged as [#1929](https://github.com/cprussin/domicile/pull/1929) and documented at the pin. [source]

## Lineage

- 2026-11-08: directional scratchpad pull and hide landed alongside focus and workspace gestures. [source] [#1929](https://github.com/cprussin/domicile/pull/1929)

## Sources

- S-9fe5a8a3f191 · 2026-09-21 · [Container commands and compositor configuration](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L1).
- S-4a2b900f6a8d · 2026-11-05 · [Scrolling layout, marks and scripting contract](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1).
- S-23c521755f22 · 2026-11-09 · [Grouped layouts, workspace homes and interaction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L1).
- S-d5b15decb594 · 2026-11-09 · [Window command vocabulary and default bindings](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L1).
- S-703c0d408b29 · 2026-11-09 · [Client visibility, suspension and buffer composition](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L1).
- S-fd0c87388c02 · 2026-11-08 · Public Domicile change catalog.
- S-e105a52826c2 · 2026-10-20 · COSMIC release source.
- S-c96f22466372 · 2026-11-09 · Swayfx release documentation.
- S-73cd44ceeaac · 2026-11-09 · Swayfx release documentation.
- S-6792066180dd · 2026-11-09 · [Native appearance and property differences](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-changes.ts#L1).
- S-8c438f028a0f · 2026-11-09 · [Desk-wide window reducer and workspace policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1).
- S-ed569f765e51 · 2026-11-09 · [Floating geometry and native-size limits](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/floating/float.ts#L1).
- S-914b44f9321b · 2026-11-09 · [Native and browser embedding element behavior](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/docs/ELEMENTS.md#L1).
