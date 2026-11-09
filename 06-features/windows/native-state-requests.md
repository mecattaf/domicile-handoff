---
id: "windows/native-state-requests"
title: "Native window state requests"
kind: "feature"
family: "windows"
layers: ["compositor", "protocol", "sdk", "manganese", "engine:blink"]
domicile_status: "landed"
domicile_refs: ["cprussin/domicile#1920", "docs/WRITING-A-SHELL.md"]
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
answered_upstream: true
scroll_parity: "baseline"
donors: ["sway", "scroll"]
eras: []
rows: ["SW-0219", "SC-0108", "SC-0363", "SC-0364", "SC-0534", "SC-0535", "SW-1069", "SW-1071", "SW-1074", "SW-1075", "SW-1078"]
depends_on: []
evidence_mix: "mostly-source"
reported_on_domicile: []
boundary_note: false
bucket: "domicile"
tags: ["family:windows", "layer:compositor", "status:landed", "evidence:source", "scroll-parity:baseline", "bucket:domicile"]
sources: ["S-e77ffff40807", "S-9fe5a8a3f191", "S-fd0c87388c02", "S-204a969601d0", "S-6792066180dd", "S-8c438f028a0f", "S-f7e6c6503485", "S-a70560fdc087", "S-ab155e5edf1e", "S-17c6354d2de5", "S-ed569f765e51", "S-9c59f8ebc0df"]
---

# Native window state requests

A native client requests maximize or fullscreen state; the shell decides placement and reports the states represented by that placement.

## Behavior

- Client requests and accepted state are separate messages.
- Changing a fullscreen or maximized state flag does not itself resize the embedded element.
- Each state update replaces shell-supplied flags while compositor-owned activation and suspension remain intact.
- Legacy clients without tiled-edge states receive the documented maximized fallback.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| sway @ pinned source | Shell state and fullscreen commands | Tiling and fullscreen are window-management states within the compositor tree. | [Window states](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L154) |
| scroll @ pinned source | Separate global, application and layout fullscreen kinds | Fullscreen allocation and client-facing fullscreen state have explicit donor modes. | [Fullscreen modes](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L561) |
| scroll @ pinned source | Fullscreen transient scene placement | A floating container uses a fullscreen scene layer when its transient-for test matches the global fullscreen or a visible region’s active fullscreen. | [Transient placement](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L1393) |

## What the notes found

- Checked 2026-11-09: The pin preserves compositor-owned activated and suspended states when the shell changes its flags. Shell-supplied native-window states can persist through page reload independently of the shell’s layout tree. [source] [State replacement and lifetime](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L469)
- Checked 2026-11-09: A new floating window uses its initial natural size. Later native-list diffs omit natural width and height, but updated min/max limits can resize floats containing one native window; multiwindow floating groups skip that clamp. [source] [Native size and limit handling](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-changes.ts#L46) · [Native size and limit handling](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1175) · [Native size and limit handling](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2574) · [Native size and limit handling](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/floating/float.ts#L201)
- Checked 2026-11-09: Native transient-parent identity and popup-parent identity are separate SDK fields. Manganese does not retain the transient relationship in its window record or use it for parent-aware placement and fullscreen policy; native descendant stacking remains unverified. [source] [Transient relationship and placement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1198) · [Transient relationship and placement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window.ts#L34) · [Transient relationship and placement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-changes.ts#L46) · [Transient relationship and placement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/placement.ts#L176)
- Checked 2026-11-09: Manganese can move a managed client to a requested live display’s current workspace before fullscreening it; exit leaves it there. Missing, removed or already-current display destinations do not trigger that move. [source] [Fullscreen destination](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindows.ts#L230) · [Fullscreen destination](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1521) · [Fullscreen destination](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2341)
- Checked 2026-11-09: Manganese maps native fullscreen and unfullscreen requests to placement actions. Maximize and unmaximize requests produce no action in its request discriminator. [source] [Request policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindows.ts#L230)
- Checked 2026-11-09: Manganese emits a close action for a missing native-window identifier, then removes window, background, scratchpad and workspace membership and matching drag state. Popup records have a separate removal path; native protocol-handle cleanup is outside this shell-state finding. [source] [Missing IDs](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-changes.ts#L18) · [Popup removal](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1290) · [Window removal](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1933)

## Where it touches Domicile

- `compositor`: A client state request is answered with the states already held and reported to the shell; the request alone changes no placement. [source] [Request handling](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L483)
- `protocol`: Clients bound below xdg_wm_base version 2 receive maximized as the tiled-state fallback when fullscreen is absent. [source] [Versioned state fallback](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L477)
- `sdk`: the request stream carries maximize, unmaximize, fullscreen and unfullscreen with the window identity; fullscreen can include a display. [source] [Typed stream](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L487)
- `manganese`: tiled windows receive four edge flags, floats receive none, and accepted fullscreen requests alter placement. [source] [Manganese states](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L493)
- `engine:blink`: the element’s CSS layout box determines size; a native-window state flag does not supply geometry. [source] [State and size](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L469)
- Status: `landed` for the typed request and state-flag contract; the requested-display fullscreen slice is merged as [#1920](https://github.com/cprussin/domicile/pull/1920), with the typed request contract documented at the pin. [source]

## Lineage

- 2026-11-07: the fullscreen request’s display target was honored by the shell. [source] [#1920](https://github.com/cprussin/domicile/pull/1920)

## Sources

- S-e77ffff40807 · 2026-11-09 · [Shell entry, client elements and native-window control](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L1).
- S-9fe5a8a3f191 · 2026-09-21 · [Container commands and compositor configuration](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L1).
- S-fd0c87388c02 · 2026-11-08 · Public Domicile change catalog.
- S-204a969601d0 · 2026-11-06 · sway and Scroll semantic inventory.
- S-6792066180dd · 2026-11-09 · [Native appearance and property differences](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-changes.ts#L1).
- S-8c438f028a0f · 2026-11-09 · [Desk-wide window reducer and workspace policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1).
- S-f7e6c6503485 · 2026-11-09 · [Window-list replay, requests and shell side effects](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindows.ts#L1).
- S-a70560fdc087 · 2026-11-09 · [Typed native-window and desktop metadata](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1).
- S-ab155e5edf1e · 2026-11-09 · [Native and browser window record model](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window.ts#L1).
- S-17c6354d2de5 · 2026-11-09 · [Window geometry, depth and fullscreen placement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/placement.ts#L1).
- S-ed569f765e51 · 2026-11-09 · [Floating geometry and native-size limits](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/floating/float.ts#L1).
- S-9c59f8ebc0df · 2026-11-05 · [Scene arrangement and animation transaction control](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L1).
