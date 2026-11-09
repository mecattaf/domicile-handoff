---
id: sway-family/scroll-parity/scratchpad-and-floating
title: Scratchpad and floating windows
kind: family
family: layout
layers: [manganese, compositor, sdk]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-0d9c2ee30def, S-204a969601d0, S-23c521755f22, S-4a2b900f6a8d, S-6b79c52fc5cd, S-8c438f028a0f, S-d5b15decb594, S-e1838c76399f]
---

# Scratchpad and floating windows

20 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 14 same; 1 scroll-changed; 5 scroll-only; 0 sway-only.
- Domicile coverage: 1 landed; 4 partial; 15 absent; 0 unverified; 0 accounting aliases/exclusions.
- Mechanism layers: compositor 3; manganese 20; sdk 4.
- Feature: [layout/floating-and-scratchpad](../../06-features/layout/floating-and-scratchpad.md).

## Donor semantics

Sway's scratchpad hides live windows and cycles them back into view. Scroll adds an overview picker for scratchpad members and optional handling of client/taskbar minimization: minimizing hides a member, while unminimizing places it into tiling again. Per-window override and global default are separate controls. [source] [Scratchpad](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L873) · [Minimize](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L881).

Manganese floats windows and complete selected groups while retaining their internal layout. Its scratchpad is modal and has a fixed screen-relative presentation. Toggle/show cycles hidden members, and directional bring/hide commands support one-way gesture actions. Scroll's jump picker and minimize-restoration behavior are wider contracts. [source] [Floating and scratchpad](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L161) · [Directional commands](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L89) · [Landed gesture slice](https://github.com/cprussin/domicile/pull/1929).

The minimize-to-scratchpad global default is off, and the per-view setting can override it. Taskbar unminimize shows the member and explicitly disables floating, so its former floating state is not restored. [source] [Global default](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L325) · [Unminimize handling](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L885).

The current scratchpad queue is ordered next-to-show first. Show restows a focused scratchpad float at the tail; otherwise it dequeues the hidden head, or leaves state unchanged if none exists. Bring does nothing when any scratchpad member is shown on the current workspace. Hide selects the focused member or falls back to the frontmost shown member. Showing adds a focused float while preserving the existing workspace fullscreen field. [source] [Queue transitions](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2482) · [One-way actions](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1538) · [Shown member](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/workspace.ts#L384) · [Float focus](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/workspace.ts#L900).

Floating movement requires membership in a workspace currently shown by a screen. The box center selects the destination screen; crossing screens rebases coordinates, moves the float to that screen’s shown workspace and focuses the screen. A center outside the desktop keeps the prior screen, while a move for an unshown workspace is ignored. These are reducer rules without a physical pointer-delivery claim. [source] [Center-based screen choice](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2270) · [Shown-workspace guard and move](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2284) · [Coordinate rebasing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2410).

## At the pin

Partial: floating groups, scratchpad show/hide and first-draw dialog handling exist; donor idempotent toggles, placement options or jump/minimize behavior extend beyond this surface. [source] [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L161) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35) · [Related change](https://github.com/cprussin/domicile/pull/1929).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-6b79c52fc5cd · 2026-11-05 · Scroll reference at the donor revision.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-e1838c76399f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
