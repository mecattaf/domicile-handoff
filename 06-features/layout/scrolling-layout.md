---
id: layout/scrolling-layout
title: Scrolling rows and columns
kind: feature
family: layout
layers: [manganese, any-shell, engine:blink]
domicile_status: absent
domicile_refs: [packages/manganese-window-management/docs/KEYS.md, packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, niri, hyprland]
eras: []
rows: [SW-0122, SW-0152, SW-0153, SW-0154, SW-0155, SW-0172, SW-0173, SW-0174, SW-0175, SC-0006, SC-0007, SC-0008, SC-0009, SC-0010, SC-0011, SC-0012, SC-0030, SC-0031, SC-0044, SC-0045, SC-0046, SC-0047, SC-0048, SC-0049, SC-0050, SC-0060, SC-0063, SC-0065, SC-0068, SC-0074, SC-0075, SC-0077, SC-0084, SC-0085, SC-0087, SC-0092, SC-0093, SW-0425, SW-0426, SW-0427, SW-0475, SW-0476, SW-0477, SW-0478, SW-0479, SW-0480, SW-0481, SW-0482, SW-0483, SW-0521, SW-0522, SW-0523, SW-0524, SW-0525, SW-0526, SW-0527, SW-0528, SW-0529, SW-0530, SW-0531, SW-0532, SW-0533, SW-0534, SW-0535, SW-0536, SC-0214, SC-0215, SC-0220, SC-0221, SC-0222, SC-0223, SC-0224, SC-0225, SC-0226, SC-0227, SC-0228, SC-0229, SC-0230, SC-0231, SC-0232, SC-0233, SC-0243, SC-0244, SC-0245, SC-0246, SC-0247, SC-0248, SC-0249, SC-0250, SC-0251, SC-0276, SC-0277, SC-0278, SC-0279, SC-0280, SC-0281, SC-0284, SC-0285, SC-0286, SC-0287, SC-0288, SC-0289, SC-0306, SC-0307, SC-0308, SC-0309, SC-0310, SC-0311, SC-0312, SC-0313, SC-0314, SC-0315, SC-0316, SC-0317, SC-0323, SC-0324, SC-0325, SC-0326, SC-0470, SC-0471, SC-0472, SC-0473, SC-0474, SC-0475, SC-0476, SC-0477, SC-0478, SC-0479, SC-0480, SC-0481, SC-0482, SC-0483, SC-0484, SC-0485, SC-0486, SC-0487, SC-0488, SC-0489, SC-0490, SC-0492, SC-0493, SC-0494, SC-0495, SC-0496, SC-0497, SC-0498, SC-0499, SC-0500, SC-0542, SC-0543, SC-0610, SC-0617, SC-0672, SC-0673, SC-0674, SC-0675, SC-0676, SC-0677, SC-0678, SC-0679, SC-0680, SC-0681]
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: false
bucket: domicile
tags: [family:layout, layer:manganese, status:absent, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-006074702454, S-23c521755f22, S-2781a84331e1, S-3533e3f0274a, S-37f72d290d44, S-48279d0d4e50, S-4a2b900f6a8d, S-578852a49413, S-5a1d66b34064, S-5e558725bd38, S-6da8864cbb54, S-7188bbbe6208, S-792ba21b119c, S-92faa6ff2e46, S-ade5027b78f6, S-b90f393c5499, S-bf5c4cf3a78c, S-c40e302fa0da, S-c7817e2e7969, S-cb1ca9702b11, S-d5b15decb594, S-e33fe2e31922, S-e77ffff40807, S-e8b20e121962]
---
# Scrolling rows and columns

A scrolling layout keeps useful window sizes while a viewport moves through columns and the windows within them.

## Behavior

- Horizontal insertion creates or joins columns; vertical insertion adds windows within a column.
- Fit, align and center change the viewport or allocation without shrinking the entire workspace into one screen.
- Jump and overview provide direct navigation through off-screen content.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| scroll @ pinned source | Rows, columns and a moving viewport | Insertion direction distinguishes a new column from a window below the current one. | [Tutorial](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L14) |
| sway @ pinned source | Finite split tree | Horizontal, vertical, tabbed and stacking containers occupy the workspace area. | [Layouts](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L178) |
| niri @ pinned source | Independent column strips per monitor | New windows keep existing sizes; each monitor owns its scrolling strip. | [Column model](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/README.md#L20) |
| Hyprland @ pinned source | Native multi-target scrolling strips | Four layout directions include a workspace-level override; fit scopes resize columns separately from focus-time camera policy. | [Strip model](https://github.com/hyprwm/Hyprland/blob/151b0434114fad482e4fed9438c081b46e73423c/src/layout/algorithm/tiled/scrolling/ScrollingAlgorithm.hpp#L27) · [Fit scopes](https://github.com/hyprwm/Hyprland/blob/151b0434114fad482e4fed9438c081b46e73423c/src/layout/algorithm/tiled/scrolling/ScrollingAlgorithm.cpp#L1315) |
| Hyprland @ pinned source | Center versus minimal camera reveal | Center uses the strip midpoint; fit clamps the existing offset, falling back to center for an oversized strip. | [Camera behavior](https://github.com/hyprwm/Hyprland/blob/151b0434114fad482e4fed9438c081b46e73423c/src/layout/algorithm/tiled/scrolling/ScrollTapeController.cpp#L252) |

## What the notes found

- Checked 2026-11-09: The donor comparison separates layout topology from individual commands; the complete command accounting is in the [scroll map](../../04-sway-family/scroll-parity/README.md). [docs]
- Scroll’s 2026-11-05 virtual-output model replaces the earlier workspace-split command. [source] [Scroll manual](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1787)
- Checked 2026-11-09: Hyprland’s native scrolling algorithm distinguishes moving the next column’s first target into the current column, expelling the last target and promoting the focused target. These target choices are separate contracts. [source] [Consume, expel and promote](https://github.com/hyprwm/Hyprland/blob/151b0434114fad482e4fed9438c081b46e73423c/src/layout/algorithm/tiled/scrolling/ScrollingAlgorithm.cpp#L1554)
- 2026-11-09: Window::SetBounds consults the parent layout manager, but Ash WindowState uses a direct helper to reach SetBoundsInternal. Maximize fullscreen and snap transitions use that path, so parent layout hooks do not intercept every bounds change (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window.cc#L617> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/layout_manager.cc#L17> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L197> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L1123> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/default_state.cc#L461> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/default_state.cc#L442>
- 2026-11-04: scroll sets default column width with layout_default_width and cycles through stored width and height fraction lists with cycle_size; defaults ship in its example config (scroll@b6586d2) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L291>
- 2026-06-11: The June capability audit separates column ordering, consume or expel, preset sizing and centering policy from window geometry, visibility and focus primitives (Chromium historical studies) [docs]
- 2026-06-29: The June uncompiled column-policy draft records that replacing requested bounds requires private WindowState access, and that the scrolling camera can conflict with existing desk-switch and overview transforms (Chromium historical studies) [docs]
- 2026-06-29: The June window policy draft separates layout manager bounds requests from direct state transition writes. An override alone misses maximize and snap; container camera transform sharing with desk and overview animators remained unresolved (Chromium historical studies) [docs]
- 2026-11-05: A 2026-11 drag survey treats drag as reversible: it serializes moves, preserves an empty source workspace, and restores placement, floating, stacking and overview state on cancellation [docs]
- 2026-11-04: miri-plugin (miracle-wm, GPL-3.0) is about 350 lines of Rust: equal half-width windows in one row, one view index per workspace, new windows at the far end; no column widths, no move or reorder. scroll is MIT and wider (miracle-wm) [docs]
- 2026-11-04: A parity map of 288 scroll semantics against Manganese, dated 2026-11-04, classed 41 present, 66 partial, 181 absent; the layout family was classed as shell-only, with no engine or compositor change involved [inferred]
- 2026-11-04: A 62-item sort, dated 2026-11-04, places scroll semantics in six classes by where a change would live: shell page code, shell package, compositor, compositor-engine interface, engine, outside Domicile [docs]
- 2026-11-06: Scroll replaces sway's split, tabbed and stacked layout modes; the notes found current scroll added virtual outputs and removed workspace splitting while its README described the older behavior (scroll) [docs]
- 2026-11-06: In the frozen sway-next prototype tabbed columns were declared but disabled and titlebar_border_thickness was dispatched while its handler returned failure; planned items were recorded apart from working ones (sway-next) [docs]

## Where it touches Domicile

- `manganese`: nested split and tab groups are documented; the scrolling-row layout is not addressed as of the pin. The absence search is the pinned command inventory and its “Differences from sway” list, together with the complete layout inventory. [source] [Commands](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L38) · [Layout inventory](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L6)
- `any-shell`: placement belongs to the shell; `<app>` supplies window embedding and CSS transforms. [source] [Shell contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L50)
- `engine:blink`: raster tilings operate below layout policy and do not implement Scroll columns. [source] [Display raster tilings](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/DISPLAY-TILINGS.md#L13)

## Lineage

- 2026-10-22: the sway-next study cataloged the Scroll layout vocabulary; the map retains its baseline semantics. [docs]

## Sources

- S-37f72d290d44 · 2026-11-05 · [Scrolling viewport and navigation semantics](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L1).
- S-4a2b900f6a8d · 2026-11-05 · [Scrolling layout, marks and scripting contract](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1).
- S-23c521755f22 · 2026-11-09 · [Grouped layouts, workspace homes and interaction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L1).
- S-d5b15decb594 · 2026-11-09 · [Window command vocabulary and default bindings](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L1).
- S-e77ffff40807 · 2026-11-09 · [Shell entry, client elements and native-window control](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L1).
- S-006074702454 · 2026-11-09 · [Multi-display raster and density selection](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/DISPLAY-TILINGS.md#L1).
- S-ade5027b78f6 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-7188bbbe6208 · 2026-11-08 · Hyprland layout, rules and focus source at the donor pin.
- S-bf5c4cf3a78c · 2026-11-08 · Hyprland layout, rules and focus source at the donor pin.
- S-48279d0d4e50 · 2026-11-08 · Hyprland layout, rules and focus source at the donor pin.
