---
id: sway-family/scroll-parity/overview
title: Overview
kind: family
family: workspaces
layers: [manganese, engine:blink]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll, niri]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-094d570afea3, S-0d9c2ee30def, S-15baf4291b31, S-204a969601d0, S-23c521755f22, S-4a2b900f6a8d, S-8c438f028a0f, S-a3bd9859358a, S-a7348ea0e59d, S-d5b15decb594, S-dae213c8d0b8]
---

# Overview

19 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 0 same; 0 scroll-changed; 19 scroll-only; 0 sway-only.
- Domicile coverage: 0 landed; 5 partial; 11 absent; 0 unverified; 3 accounting aliases/exclusions.
- Mechanism layers: engine:blink 19; manganese 19.
- Feature: [workspaces/overview](../../06-features/workspaces/overview.md).

## Donor semantics

Scroll exposes two live views: a fitted view of one workspace and a per-output view of all its workspaces. Normal application interaction remains active, including typing, creation, closing, focus and movement. Workspace zoom is also independently adjustable. These are broader contracts than a selection-only overview. [source] [Workspace scaling](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L857).

The donor rows distinguish preview-coordinate mapping, workspace-specific hit testing, output clipping, scale restoration and empty-workspace lifetime during the global view. Each invariant has a source anchor in the row table. [source] [Coordinate mapping](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L502).

The donor's centering and reordering overrides have different state effects. During per-workspace overview, centering getters return false without changing their stored flags. The reorder getter assigns automatic reordering to the workspace modifier before returning it. The source therefore does not describe both operations as an identical temporary read-only override. [source] [Modifier getters](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L697) · [Overview save and restore](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L327).

## Domicile comparison

Manganese uses live transformed windows in workspace tiles and supports picking and dragging. Its overview translates directional focus into tile aiming and directional movement into workspace swaps. Other window/workspace commands are suppressed while it is open. That command boundary differs from Scroll's normally editable overview. [source] [Current behavior](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L333) · [Overview steering](https://github.com/cprussin/domicile/pull/1946).

The current picker first resolves a tile under the pointer, then compares transformed frame rectangles from that workspace in descending stacking order. This establishes tile-scoped picking; ordinary client input through an editable preview is a different contract. Tile fitting caps the workspace image at three fifths of screen width, including the single-workspace case. The tile list comes from current workspace homes; after each reduction, an empty hidden workspace loses its home without an overview exception. Fixed workspace identities remain available. [source] [Picker scope](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/overview/overview.ts#L61) · [Scale bound](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/overview/tiles.ts#L131) · [Home cleanup](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2533).

## Additional donor boundary

Ash transforms live windows during overview, tracks transient descendants and can mirror minimized content. Its managed windows have ordinary targeting blocked for the overview interval, then restored. This differs from Scroll's normally interactive scaled clients and from Manganese's tile picker. Ash controller dependencies remain a Chromium donor reference rather than an available Linux overview controller. [source] [Transform lifetime](https://github.com/chromium/chromium/blob/150.0.7871.124/ash/wm/overview/scoped_overview_transform_window.h#L37) · [Targeting](https://github.com/chromium/chromium/blob/150.0.7871.124/ash/wm/overview/scoped_overview_transform_window.h#L206).
Niri's overview retains the ordinary keyboard shortcuts and supports moving windows across existing workspaces or dropping between them to create a workspace. That command scope differs from Manganese's documented overview command suppression. [source] [Niri overview](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Overview.md#L14) · [Workspace creation by drop](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Overview.md#L36).

Per-workspace overview saves member positions, workspace scale and the earlier fullscreen member. Exit restores positions and scale; fullscreen restoration checks current focus and the fullscreen-follow policy. A changed focus can therefore replace or clear the earlier fullscreen relation. [source] [Overview exit](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L327).

Global overview uses a separate path. Its pointer mapper searches every output’s preview rectangles with half-open bounds, then converts the selected point to workspace coordinates; a point in a gap produces no workspace target. Exit clears workspace scene tags, restores still-recorded fullscreen references and requests empty-workspace cleanup. [source] [Pointer mapping](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L502) · [Global teardown](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L469).

Manual workspace exact/increment zoom clamps to 0.2–1.0. Reset disables scaling, while exact 1.0 keeps the positive-scale predicate enabled. Automatic overview fitting uses a separate cap at one without the manual lower clamp. The per-workspace overview command reports success without toggling while jump is active. [source] [Manual command](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/overview.c#L53) · [Reset and predicate](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L568) · [Automatic fit](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L214).

The overview aim is retained only while overview is open and its target workspace has a home. Otherwise it is cleared so steering returns to the workspace currently shown. This target lifetime is distinct from preserving the fixed workspace names or native client identity. [source] [Aim validation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1222).

Overview toggle closes the launcher and clipboard panel and clears active drag state. Explicit show delegates to that toggle only when overview was closed, so showing an already-open overview preserves its state. [source] [Toggle effects](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1486) · [Idempotent show](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1491).

## At the pin

Not addressed at the pin: arbitrary per-workspace visual zoom and a separate per-workspace overview command are not in the complete command/state inventory; the shipped overview shows fitted tiles across screens. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L121).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-094d570afea3 · 2026-11-05 · Scroll reference at the donor revision.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-15baf4291b31 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-a3bd9859358a · 2026-11-08 · Niri reference at the donor revision.
- S-a7348ea0e59d · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-dae213c8d0b8 · 2026-11-05 · Scroll reference at the donor revision.
