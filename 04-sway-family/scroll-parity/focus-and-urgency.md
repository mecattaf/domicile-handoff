---
id: sway-family/scroll-parity/focus-and-urgency
title: Focus and urgency
kind: family
family: windows
layers: [manganese, compositor, protocol]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-0d9c2ee30def, S-17c6354d2de5, S-204a969601d0, S-23c521755f22, S-4a2b900f6a8d, S-6b79c52fc5cd, S-80a29ef26559, S-8c438f028a0f, S-a70560fdc087, S-c0a5420d3a23, S-d5b15decb594, S-dae213c8d0b8, S-e1838c76399f, S-fbe8a9d8422d]
---

# Focus and urgency

79 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 52 same; 10 scroll-changed; 17 scroll-only; 0 sway-only.
- Domicile coverage: 2 landed; 8 partial; 68 absent; 0 unverified; 1 accounting aliases/exclusions.
- Mechanism layers: compositor 79; manganese 78; protocol 1.
- Feature: [windows/focus-and-urgency](../../06-features/windows/focus-and-urgency.md).

## Donor semantics

Focus contracts include directional tree traversal, parent/child selection, layer selection, pointer focus, output escape, activation policy, urgency and pointer warping. Scroll adds an explicit stay policy and a creation-ordered focus ring whose membership can be edited; this ring is distinct from recent-focus restoration. [source] [Focus ring](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L542) · [Wrapping](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1580).

Manganese documents pointer focus without hover raising, restores recent focus after close and traverses to neighboring screens with directional keys. Activation reveals the target workspace and tab. It does not expose the complete donor set of focus/ring/urgency configuration controls. [source] [Current focus behavior](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L64) · [Native urgency](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1211).

The focus-ring source inserts a newly mapped view immediately after its navigation cursor; explicit set removes an earlier occurrence and appends the view at the end. A positive length bounds navigation to the stored list's suffix without trimming that list. Wrapping is disabled by default, and ring navigation reveals a hidden scratchpad target before focusing it. Unmapping removes the live view reference. [source] [Navigation and length](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/focus_ring.c#L22) · [Membership](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/focus_ring.c#L100) · [Unmap cleanup](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1145) · [Default wrapping](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config.c#L299).

The layer toggle selects the last-raised floating group when leaving tiling and restores remembered tiling focus on return; an empty destination layer leaves the workspace unchanged. Directional focus can enter an empty neighboring screen before wrapping. Floating focus never escapes through that path; local fullscreen permits screen escape, while global fullscreen does not. [source] [Layer focus](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/workspace.ts#L445) · [Screen traversal](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1730) · [Fullscreen bounds](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/workspace.ts#L469).

Scroll’s focus activation policy explicitly reveals a hidden scratchpad target. Smart activation focuses a target only when its workspace is visible, otherwise marking it urgent. Manganese’s focus-request helper can reveal a registered background window, then reaches the target through workspace membership. That helper does not dequeue the scratchpad, and a request matching the recorded focused identifier leaves state unchanged. These are shell-policy bounds; native activation-token validation and lock gates are separate. [source] [Donor activation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L540) · [Current request helper](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1946) · [Hidden background reveal](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1908) · [Workspace requirement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1980).

The pinned guide and reducer differ on hover raising. The WindowHovered path ignores dragging and hidden workspaces and skips an already-focused target on its screen. Otherwise it reaches the target; reaching a float moves its group to the end of ordinary floating order, which placement assigns the greatest ordinary float depth within that workspace’s floating list. This is a conditional source path, not proof of every physical/native pointer event reaching it. [source] [Published policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L71) · [Hover dispatch](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1627) · [Current-focus guard](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2062) · [Float order](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/workspace.ts#L279) · [Placement depth](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/placement.ts#L186).

Scroll’s full-visibility query first applies ordinary visibility and allowed fullscreen/transient exceptions. Other tiled targets use workspace output-area or physical-output containment; floats use the desktop box. It then rejects rectangular overlap with later floats in the same workspace, explicitly leaving cross-output floating overlap unhandled. This is separate from the layout visible filter, which checks container bounds without calling that floating-overlap test. [source] [Complete query](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1359) · [Layout filter](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L1854).

During document-to-native keydown handling, a missing app element clears the renderer’s recorded native keyboard target. With a bound channel it requests keyboard focus for the page. This is a source-level recovery request rather than a new physical focus-delivery result. [source] [Missing-element recovery](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1281).

## At the pin

Directional focus traverses the current layer and can enter a neighboring screen before wrapping. Empty screens are valid targets; floats and global fullscreen do not leave their workspace through this path. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L67) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1730) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/workspace.ts#L463) · [Related change](https://github.com/cprussin/domicile/pull/1892).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-17c6354d2de5 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-6b79c52fc5cd · 2026-11-05 · Scroll reference at the donor revision.
- S-80a29ef26559 · 2026-11-05 · Scroll reference at the donor revision.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-a70560fdc087 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-c0a5420d3a23 · 2026-11-05 · Scroll reference at the donor revision.
- S-dae213c8d0b8 · 2026-11-05 · Scroll reference at the donor revision.
- S-e1838c76399f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-fbe8a9d8422d · 2026-11-09 · Domicile shell and typed-state reference at the pin.
