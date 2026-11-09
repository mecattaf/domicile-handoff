---
id: sway-family/scroll-parity/scrolling-rows-and-columns
title: Scrolling rows and columns
kind: family
family: layout
layers: [any-shell]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll, hyprland]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-0d9c2ee30def, S-1fa1acbff487, S-204a969601d0, S-23c521755f22, S-2a7c4133bfd5, S-37f72d290d44, S-38ebcfd69071, S-4a2b900f6a8d, S-7188bbbe6208, S-8c438f028a0f, S-9c59f8ebc0df, S-ab9a24bc3733, S-bf5c4cf3a78c, S-d5b15decb594, S-dae213c8d0b8, S-ea67ad0bc668]
---

# Scrolling rows and columns

114 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 37 same; 15 scroll-changed; 62 scroll-only; 0 sway-only.
- Domicile coverage: 0 landed; 21 partial; 93 absent; 0 unverified; 0 accounting aliases/exclusions.
- Mechanism layers: any-shell 114.
- Feature: [layout/scrolling-layout](../../06-features/layout/scrolling-layout.md).

## Donor semantics

A horizontal workspace contains a sequence of columns. Each column holds windows on its other axis. Windows keep chosen dimensions when the sequence exceeds the viewport; focusing an offscreen item moves the viewport. Vertical output orientation transposes this organization. [source] [Rows and columns](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L14).

Working mode and output orientation are separate variables. Horizontal insertion starts a new primary-axis group; vertical insertion adds inside the current group, with the interpretation transposed on vertical outputs. Each workspace stores insertion position, fitting, creation focus, centering, viewport reordering and floating defaults. Global defaults are overlaid by output defaults and then workspace defaults. [source] [Mode controls](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L926) · [Default precedence](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1855).

- Position chooses before, after, beginning or end.
- `nofit` permits overflow; `fitsplit` divides the active item's allocation; `fitfraction` redistributes the fitted group.
- Movement can carry a group or one window according to working mode. `nomode` bypasses that choice.
- Width and height presets are fractions of usable viewport space, with direct setting and cycling as separate operations.

These are separate source contracts, rather than aliases for sway's arbitrary split tree. [source] [Insertion](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L926) · [Movement](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L684) · [Size fractions](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L52).

## Viewport placement

The ordinary placement routine evaluates edge-aligned candidates using the actual widths/heights. Eligible candidates keep the active item fully visible and avoid unnecessary empty viewport space; the selected candidate minimizes movement of that item. Centering and pin rules are applied separately. Gesture scrolling, lazy reorder and jump can preserve the current position instead of running that selection. [source] [Candidate evaluation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L506) · [Overrides](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L696).

At the current pin, Manganese's complete command parser exposes only fixed directional growth: a tiled window changes by one fiftieth of its container and a float by ten pixels. It has no shrink action, absolute resize setter, width/height-only form, or caller-supplied pixel/percentage amount. The lower-level SDK can set bounds, which is a separate programming primitive from the Manganese command forms not addressed at the pin. [source] [Exact command forms](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L93) · [Whole-command lookup](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L179) · [Step sizes](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L72) · [Bounds primitive](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/windows.ts#L38).

The live workspace-mode parser applies recognized tokens in their written order. A later token for the same setting replaces the earlier value; at least one recognized token produces success even if other tokens are unrecognized. Mode/insertion changes update the addressed container, while fit and centering changes request arrangement. The directional-move helper treats a pixel amount and `nomode` as alternatives in the first optional argument slot. `nomode` first resolves the active leaf; it can detach that leaf from a group or insert it in a neighbor according to direction. [source] [Live modifier parser](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/set_mode.c#L26) · [Move grammar](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/move.c#L492) · [Leaf movement](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L1599).

## Lineage

- 2026-10-22: a donor anatomy study distinguishes spatial policy, animation state, client density and inverse input mapping. Its source-size and interface counts describe that older checkout. [docs] S-2a7c4133bfd5 · 2026-10-22 · Scroll mechanism anatomy.
- 2026-11-04: a parity study compares scrolling groups with Manganese trees. Its historical status cells are superseded by the current-pin checks in this map. [docs] S-38ebcfd69071 · 2026-11-04 · scrolling-layout semantic comparison.

## Other donor contracts

Hyprland has a native scrolling algorithm with multi-window strips and four directions, including a workspace-specific direction override. Consume takes the next column's first member; expel extracts the current column's last member; promote extracts the focused member. These are distinct membership operations. [source] [Native model](https://github.com/hyprwm/Hyprland/blob/151b0434114fad482e4fed9438c081b46e73423c/src/layout/algorithm/tiled/scrolling/ScrollingAlgorithm.hpp#L27) · [Direction](https://github.com/hyprwm/Hyprland/blob/151b0434114fad482e4fed9438c081b46e73423c/src/layout/algorithm/tiled/scrolling/ScrollingAlgorithm.cpp#L1953) · [Membership operations](https://github.com/hyprwm/Hyprland/blob/151b0434114fad482e4fed9438c081b46e73423c/src/layout/algorithm/tiled/scrolling/ScrollingAlgorithm.cpp#L1554).

The per-workspace creation mode controls insertion, fitting and focus as well as orientation. Its floating policy applies only to newly mapped windows, leaves existing members placed and runs before later criteria rules. Automatic reordering is enabled by default; alignment disables it until explicitly restored. [docs] [Workspace policies](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L926).

Drag/drop chooses whether to move the whole primary group or extract a single view from the source workspace’s working mode. The destination orientation determines which edges insert into a group and which edges create a neighboring group. Dropping on a distinct target without an edge swaps the relevant groups or views. Cross-workspace group movement changes the group’s internal orientation to remain orthogonal to the destination layout. [source] [Workspace dragging](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L1171) · [Target-edge behavior](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L1404).

## At the pin

Not addressed at the pin: the complete layout/state and command inventory describes nested split/tabbed/stacking trees, with no unbounded scrolling row/column policy. [source] [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L6) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L121) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-1fa1acbff487 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-2a7c4133bfd5 · 2026-11-09 · donor source accounting and current-state evidence.
- S-37f72d290d44 · 2026-11-05 · Scroll reference at the donor revision.
- S-38ebcfd69071 · 2026-11-09 · donor source accounting and current-state evidence.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-7188bbbe6208 · 2026-11-08 · Hyprland scrolling reference at the donor revision.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-9c59f8ebc0df · 2026-11-05 · Scroll reference at the donor revision.
- S-ab9a24bc3733 · 2026-11-05 · Scroll reference at the donor revision.
- S-bf5c4cf3a78c · 2026-11-08 · Hyprland scrolling reference at the donor revision.
- S-dae213c8d0b8 · 2026-11-05 · Scroll reference at the donor revision.
- S-ea67ad0bc668 · 2026-11-05 · Scroll reference at the donor revision.
