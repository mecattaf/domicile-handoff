---
id: layout/tiling-splits-and-containers
title: Nested splits and container layouts
kind: feature
family: layout
layers: [manganese, sdk, engine:blink]
domicile_status: landed
domicile_refs: [cprussin/domicile#1891, packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, niri, macos]
eras: []
rows: [SW-0128, SW-0143, SW-0145, SW-0146, SW-0147, SW-0148, SW-0179, SW-0180, SW-0181, SW-0182, SW-0183, SW-0229, SC-0034, SC-0071, SC-0105, SW-0428, SW-0429, SW-0430, SW-0458, SW-0459, SW-0460, SW-0461, SW-0462, SW-0463, SW-0464, SW-0465, SW-0466, SW-0467, SW-0468, SW-0469, SW-0470, SW-0471, SW-0472, SW-0473, SW-0474, SW-0537, SW-0538, SW-0539, SW-0540, SW-0541, SW-0542, SW-0543, SW-0544, SW-0628, SW-0629, SW-0630, SC-0218, SC-0219, SC-0264, SC-0265, SC-0266, SC-0357, SC-0358, SC-0536, SC-0537, SW-1080, SW-1145, SC-0643]
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: false
bucket: domicile
tags: [family:layout, layer:manganese, status:landed, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-0d7150dbb6e9, S-23c521755f22, S-37f72d290d44, S-52d31a0991c7, S-612a99e2b687, S-792ba21b119c, S-83c688ce4204, S-8c438f028a0f, S-8e001f804b21, S-9883240b5481, S-9be40f33ce62, S-9fe5a8a3f191, S-a59107f9d164, S-a5c59a9cf1ff, S-de5ea9e2cd26, S-e77ffff40807, S-ecfec2f17332, S-f700add0ab88, S-f7e6c6503485]
---
# Nested splits and container layouts

A layout tree groups windows into splits, tabs and stacks, with focus and movement defined at each level.

## Behavior

- A selected container can be a window or a group; parent selection changes which unit a command acts on.
- Groups preserve internal structure when moved, floated or put into another group.
- Changing layout and inserting a new window are separate operations.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| sway @ pinned source | Container tree and layout commands | A split sets insertion orientation; tabbed and stacking present one child at a time. | [Layout commands](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L178) |
| scroll @ pinned source | Column-oriented replacement of the sway tree | The donor retains related navigation vocabulary while changing the layout model. | [Rows and columns](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L14) |
| niri @ pinned source | Tabbed column presentation | The column retains the same window-navigation and consume/expel operations in tabbed display. | [Tabbed columns](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Tabs.md#L5) |
| AppKit @ 10.13 | Application-owned window tab groups | Selection, detaching, merging and tab overview are distinct operations. [docs] | [Versioned release notes](https://developer.apple.com/library/archive/releasenotes/AppKit/RN-AppKit/index.html) |

## What the notes found

- Checked 2026-11-09: The same command name can describe different tree edits: Manganese’s split moves an existing window out of a group; sway’s split changes how the next insertion is arranged. [source] [Manganese difference](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L30) · [sway split](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L351)
- Checked 2026-11-09: Manganese containers hold children, layout, last-focused child and size fractions without an independent container identifier. A structural reference combines a window ID with an ancestor count and is validated against the current tree. [source] [Group record and current-tree reference](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/tree/node.ts#L28) · [Group record and current-tree reference](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/tree/path.ts#L13)
- Checked 2026-11-09: Manganese kill targets every window in the selected node. Native and browser closes are requests; registered background application windows hide instead. Overview guards can suppress the keyboard action. [source] [Selected-node close](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L328) · [Selected-node close](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindows.ts#L123) · [Selected-node close](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindows.ts#L290)
- 2026-11-09: Native maximize and fullscreen requests are separate from granted window states; the compositor grants none until the shell calls setWindowStates [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/window-state.ts#L27> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/system-calls.ts#L146>
- 2026-11-04: Domicile docs state that every window sits in a tab group, a new window opens as a tab, and split h/v moves a window out of its group [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L55> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L165>
- 2026-11-09: Manganese tiled allocation subtracts two 20-pixel horizontal insets and 30 pixels vertically before individual window title-bar allocation [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/gaps.ts#L23>
- 2026-11-05: Manganese resize grow moves a tiled window by one fiftieth of its container and a floating window by ten pixels, and is unbound by default; sway-style percentage-point resizing is not documented [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L72>
- 2026-06-11: A 2026-06-11 design separates geometry from native fullscreen state: a full-output rectangle does not itself perform the configure handshake expected by a Wayland client (ash@2026-06-11) [docs]
- 2026-06-11: A June study separates MiracleWM’s native tiling tree from placement advice and its external command socket; command access does not transfer ownership of the layout model (miracle-wm@2026-06-study) [docs]

## Where it touches Domicile

- `manganese`: nested tabs, stacks, parent selection and group moves are present at the pin. [source] [Tree and commands](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L14)
- `sdk`: The window namespace supplies live client identities and metadata to the shell’s layout model. [source] [Client-window list](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L368)
- `engine:blink`: Built-in window elements embed clients in the page; an `<app>` layout box determines the native client’s size. [source] [Built-in elements](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L56) · [Layout size](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L403)
- Status: `landed`; the current behavior is documented at the pin. The nested-group interaction slice is merged as [#1891](https://github.com/cprussin/domicile/pull/1891). [source]

## Lineage

- 2026-10-22: the sway-next tabbed-column study distinguished tree state from the later interaction layer. [docs]

## Sources

- S-9fe5a8a3f191 · 2026-09-21 · [Container commands and compositor configuration](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L1).
- S-37f72d290d44 · 2026-11-05 · [Scrolling viewport and navigation semantics](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L1).
- S-23c521755f22 · 2026-11-09 · [Grouped layouts, workspace homes and interaction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L1).
- S-e77ffff40807 · 2026-11-09 · [Shell entry, client elements and native-window control](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L1).
- S-0d7150dbb6e9 · 2026-11-08 · Niri tabbed-column contract at the donor revision.
- S-de5ea9e2cd26 · 2017-08-24 · AppKit release documentation.
- S-52d31a0991c7 · 2026-11-09 · [Current-tree references and ancestor resolution](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/tree/path.ts#L1).
- S-a5c59a9cf1ff · 2026-11-09 · [Nested group records and layout structure](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/tree/node.ts#L1).
- S-8c438f028a0f · 2026-11-09 · [Desk-wide window reducer and workspace policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1).
- S-f7e6c6503485 · 2026-11-09 · [Window-list replay, requests and shell side effects](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindows.ts#L1).
