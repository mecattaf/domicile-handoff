---
id: layout/layout-retention-across-reload
title: Window placement across shell reloads
kind: feature
family: layout
layers: [any-shell, manganese, host]
domicile_status: known-gap
domicile_refs: [docs/KNOWN-GAPS.md, docs/CLI.md, docs/WRITING-A-SHELL.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway]
eras: []
rows: []
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: [L-01]
boundary_note: false
bucket: domicile
tags: [family:layout, layer:any-shell, status:known-gap, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-3674591ca8e6, S-3fb9770260b2, S-446385a8309f, S-4513dd8587bb, S-598603ed3fbd, S-695f43c6be84, S-782e09e7d435, S-8100b35ed50a, S-9fe5a8a3f191, S-e77ffff40807, S-f7e6c6503485]
---
# Window placement across shell reloads

A shell reload preserves client windows while replacing the page that holds the shell’s layout state.

## Behavior

- Client lifetime and shell-page lifetime are separate.
- Placement retained only in page memory is lost when that page reloads.
- Reconstructing or retaining layout requires a shell state contract beyond the live window list.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| sway @ pinned source | Configuration reload inside a persistent compositor | Reload reads configuration without replacing the window-management process. | [Reload command](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L312) |

## What the notes found

- Checked 2026-11-09: The pin records loss of state held by the replaced page. [source] [Shell reload gap](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md#L76)
- Checked 2026-11-09: Client-facing tiled/fullscreen state can outlive a shell reload while the layout page is replaced; retained protocol state does not establish retained workspace placement. [source] [State lifetime](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L469)
- Checked 2026-11-09: A replacement shell adopts the existing native surface. Changing its box size or scale allocates a new local surface identity for frames at that size; this embedding lifecycle is separate from workspace reconstruction. [source] [Surface adoption](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/platform/graphics/external_surface_embedder.cc#L65)
- Checked 2026-11-09: Wayland-protocols 1.49 defines application-session restoration for native toplevels and leaves remembered state and eviction policy to the compositor. That client-session protocol has a different lifetime from replacing a shell page. [source] [Application-session contract](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/xdg-session-management/xdg-session-management-v1.xml#L60)
- 2026-11-05: scroll defines a space as a configuration of existing windows that can be loaded onto any workspace, hiding non-members; the README restores layouts by loading a space (scroll@b6586d2) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L993>
- 2026-11-06: On a shell reload every window stayed open but workspace placement was not retained; every window landed on workspace 1 (living row L-01). [measured]

## Where it touches Domicile

- `host`: clients stay open through `load-shell`, and a shell that does not run can return to its predecessor. [source] [Load-shell behavior](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/CLI.md#L33)
- `any-shell`: Window placement is supplied by the shell’s layout of client elements. [source] [Shell placement contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L50)
- `manganese`: The window hook starts from a fresh reducer state and replays existing native and browser-window lists; it does not load a saved workspace tree. [source] [Reducer initialization](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindows.ts#L73) · [Live-list replay](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindows.ts#L149)
- Status: `known-gap`; page-held state loss is explicit at the pin. [source] [Gap](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md#L76)

## Lineage

- 2026-11-09: the pin documents client preservation and shell-page state loss as separate lifecycle facts. [source] [Reload boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/HOW-IT-WORKS.md#L94)

## Sources

- S-9fe5a8a3f191 · 2026-09-21 · [Container commands and compositor configuration](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L1).
- S-8100b35ed50a · 2026-11-09 · [Documented desktop boundaries at the pin](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md#L1).
- S-782e09e7d435 · 2026-11-09 · [Shell loading and command dispatch](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/CLI.md#L1).
- S-e77ffff40807 · 2026-11-09 · [Shell entry, client elements and native-window control](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L1).
- S-3fb9770260b2 · 2026-11-09 · [Host, compositor and browser integration](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/HOW-IT-WORKS.md#L1).
- S-4513dd8587bb · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-3674591ca8e6 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-f7e6c6503485 · 2026-11-09 · [Window-list replay, requests and shell side effects](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindows.ts#L1).
