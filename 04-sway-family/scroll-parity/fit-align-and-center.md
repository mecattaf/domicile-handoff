---
id: sway-family/scroll-parity/fit-align-and-center
title: Fit, align and center
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
sources: [S-0d9c2ee30def, S-18d5a6931a7b, S-204a969601d0, S-23c521755f22, S-36bae2cd0f28, S-48279d0d4e50, S-4a2b900f6a8d, S-8c438f028a0f, S-9c52ccf3ef99, S-bf5c4cf3a78c, S-c0a5420d3a23, S-cb8bbffa001b, S-d5b15decb594, S-dae213c8d0b8, S-db8d6a3c9769, S-e7b06d89358d]
---

# Fit, align and center

48 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 1 same; 0 scroll-changed; 47 scroll-only; 0 sway-only.
- Domicile coverage: 0 landed; 0 partial; 48 absent; 0 unverified; 0 accounting aliases/exclusions.
- Mechanism layers: any-shell 48.
- Feature: [layout/scrolling-layout](../../06-features/layout/scrolling-layout.md).

## Donor semantics

`fit_size` chooses an axis, a scope and an allocation rule. Scopes select the active item, all intersecting visible items, every sibling, or the range between focus and either end. Visible scope includes items cut by the viewport edge. Proportional allocation retains relative dimensions; equal allocation replaces those ratios. [source] [Fit contract](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L487).

Automatic alignment reveals focus and attempts to retain an adjacent item. Manual alignment anchors to either edge or the center. `middle` centers both the primary group and the focused window inside it. An independent reset policy determines whether a focus change restores automatic alignment. Persistent horizontal and vertical centering are workspace mode modifiers. [source] [Align](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L417) · [Reset](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L63) · [Persistent centering](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L960).

Preset cycling, direct fractional size and temporary sizing are distinct controls. Temporary active/all sizing remembers earlier dimensions, exempts windows resized manually and adopts the destination workspace's mode after a move. [source] [Direct sizing](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L977) · [Temporary sizing](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1069).

The fit command rejects a direct floating target and a hidden scratchpad target. Its axis chooses either workspace siblings or members of the current group according to workspace orientation. Selection uses scaled intersections, while new dimensions and gap-aware fractions use the unscaled workspace extent. Overview/fullscreen guards return without fitting. [source] [Target and axis](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/fit_size.c#L6) · [Allocation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L4770) · [Guards](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L4947).

Manual center follows the current mode axis; middle combines both axes. Alignment returns without moving during overview, and reset restores the workspace alignment state. [source] [Center and middle](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/align.c#L95).

Preset cycling scans the declared list in order, using a 0.001 comparison tolerance. Wrapping is off by default; when enabled, an exhausted scan selects the first or last entry. Floating cycling derives a fraction from current geometry, applies floating constraints and keeps the window center; tiled cycling changes stored fractions and can operate on the parent group along the workspace axis. The numeric-array parser preserves declaration order. [source] [Cycling](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/cycle_size.c#L16) · [Floating](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/cycle_size.c#L55) · [Tiled](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/cycle_size.c#L101) · [List parser](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/common/util.c#L300) · [Defaults](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config.c#L282).

Persistent fit modes initialize new fractions differently: no-fit uses configured defaults, split-fit halves the reference fraction, and fraction-fit starts from the existing average weight. With fitting enabled, a changed fraction rescales peer weights toward the remaining share with a small positive floor; fullscreen/overview guards exclude that adjustment. [source] [Initial fractions](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L895) · [Peer adjustment](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L4993).

Temporary sizing has two levels: the workspace's active/all policy and explicit per-container toggles. Ordinary focus changes save and restore fractions; a manual resize creates an exemption that persists across later focus changes. The `this` operation applies to the selected container and its parent without changing the workspace policy. Reset removes that policy while explicit per-container overrides keep their own state. Floating size backups additionally retain position and dimensions. [source] [State transitions](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L4195) · [Workspace policy](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L4502) · [Container toggle](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L4626) · [Floating backup](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L4156).

The pinned command parser first resets the workspace policy. Repeating the same mode reinstates it only when both supplied fractions differ beyond its comparison tolerance; changing just one fraction therefore leaves the policy reset. Separately, the single-item maximization trigger counts top-level tiling groups, so one group containing several windows meets that test. These statements describe the current source predicates. [source] [Toggle predicate](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/toggle_size.c#L16) · [Single-group trigger](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L4543).

## Other donor contracts

Hyprland exposes active, expand, all, to-end, to-beginning and visible fit scopes. Resizing the selected scope is separate from choosing camera centering or fitting during focus changes. Centering aligns a strip midpoint; fitting clamps the existing offset to expose it and centers only when the strip exceeds the viewport. [source] [Fit scopes](https://github.com/hyprwm/Hyprland/blob/151b0434114fad482e4fed9438c081b46e73423c/src/layout/algorithm/tiled/scrolling/ScrollingAlgorithm.cpp#L1315) · [Focus-time policy](https://github.com/hyprwm/Hyprland/blob/151b0434114fad482e4fed9438c081b46e73423c/src/layout/algorithm/tiled/scrolling/ScrollingAlgorithm.cpp#L413) · [Camera adjustment](https://github.com/hyprwm/Hyprland/blob/151b0434114fad482e4fed9438c081b46e73423c/src/layout/algorithm/tiled/scrolling/ScrollTapeController.cpp#L252).

Manual workspace scale and overview are distinct guard states. Fit entry points test fullscreen/overview state, while layout fullscreen additionally rejects a positive workspace scale, including exact 1.0. Reset clears that positive-scale predicate. [source] [Fit guards](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L4947) · [Layout fullscreen guard](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/container.c#L1748) · [Scale predicate](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L584).

## At the pin

Not addressed at the pin: there is no center-position command in the complete parser; opening certain dialog windows centered is a narrower policy. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L121).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-18d5a6931a7b · 2026-11-05 · Scroll reference at the donor revision.
- S-36bae2cd0f28 · 2026-11-05 · Scroll reference at the donor revision.
- S-48279d0d4e50 · 2026-11-08 · Hyprland scrolling reference at the donor revision.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-9c52ccf3ef99 · 2026-11-05 · Scroll reference at the donor revision.
- S-bf5c4cf3a78c · 2026-11-08 · Hyprland scrolling reference at the donor revision.
- S-c0a5420d3a23 · 2026-11-05 · Scroll reference at the donor revision.
- S-cb8bbffa001b · 2026-11-05 · Scroll reference at the donor revision.
- S-dae213c8d0b8 · 2026-11-05 · Scroll reference at the donor revision.
- S-db8d6a3c9769 · 2026-11-05 · Scroll reference at the donor revision.
- S-e7b06d89358d · 2026-11-05 · Scroll reference at the donor revision.
