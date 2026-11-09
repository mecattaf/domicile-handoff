---
id: sway-family/scroll-parity/animations-and-feel
title: Animations and feel
kind: family
family: feel-and-animation
layers: [manganese, engine:blink, compositor]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll, niri]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-0d9c2ee30def, S-0e5ba5f15aa1, S-12e11cee7f48, S-1f455f8b0a46, S-204a969601d0, S-23c521755f22, S-26338d5a2c95, S-4a2b900f6a8d, S-4cf15b4011fd, S-4f20a12f585a, S-5d34e53f820b, S-70ac6562eb03, S-7f2d34d0bfe7, S-9c59f8ebc0df, S-ab6f88da4d6c, S-b23d6954c82b, S-c0ee57a62724, S-d5b15decb594, S-e77ffff40807]
---

# Animations and feel

179 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 100 same; 9 scroll-changed; 69 scroll-only; 1 sway-only.
- Domicile coverage: 0 landed; 14 partial; 147 absent; 2 unverified; 16 accounting aliases/exclusions.
- Mechanism layers: compositor 1; engine:blink 179; manganese 179.
- Feature: [feel-and-animation/scroll-feel-parity](../../06-features/feel-and-animation/scroll-feel-parity.md).

## Donor semantics

Scroll separates operation timing from spatial policy. Each operation can have its own duration, changing-variable curve and independent off-axis curve. General curves use arc-length parameterization and permit higher orders and loops; an optional simple cubic follows CSS timing conventions. [source] [Curve format](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L136).

Its resize styles differ: clipping keeps client pixels at their resolution while the visible box changes, whereas scaling stretches the contents with the animated box. Source rows also record interruption retargeting, shared-axis motion, output removal and retained content during exit. [source] [Resize styles](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L75) · [Animation declarations](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/include/sway/desktop/animation.h#L1).

Manganese has operation animations and a coordinated wrapper/content settling path, but no matching Scroll curve-command surface. It configures a resized client once and animates transforms around that result. Normal close retains visual content for the leaving interval; this is distinct from every possible client unmap transition. [source] [Settling](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L410) · [Retained close](https://github.com/cprussin/domicile/pull/1910) · [Closing tabs](https://github.com/cprussin/domicile/pull/1939).

Per-binding suppression is applied after command execution and the successful binding event, to the resulting dirty transaction. If there is no dirty work, no transaction is marked. The queued transaction temporarily disables animation setup, then restores the earlier configured enable state. A later jump-selection callback or command from a spawned script runs in a separate transaction. [source] [Binding execution](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/bind.c#L638) · [Transaction scope](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L2108).

Niri separates easing from spring animation. Springs consume swipe-release velocity, and their duration follows the physical parameters rather than a fixed duration. Resize-driven camera and sibling motion can use the resize timing configuration; the reference also records actions whose synchronization is incomplete. Its clock model distinguishes input-time starts from predicted presentation-time sampling and shares a cached timestamp during one event iteration. [source] [Gesture velocity](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Configuration:-Animations.md#L110) · [Related motion](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Configuration:-Animations.md#L457) · [Clock model](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Development:-Animation-Timing.md#L26).

Manganese's floating-window shadow explicitly ignores DOM pointer events. Its settling path reads the eased animation progress to find the displayed box before retargeting, then cancels the earlier animation and starts the replacement from that box. These component facts do not establish every native input-coordinate transition or the donor's separate decoration pass-through margin. [source] [Shadow targeting](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/floating/FloatShadow.tsx#L87) · [Interruption geometry](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useSettling.ts#L283) · [Progress and replacement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useSettling.ts#L325).

The per-operation animation block is configuration-only; the separate global enable command is available in the ordinary command table. An unset operation path inherits the default path, while an explicitly disabled path stays disabled. Configuration reload also suppresses enabled paths. [source] [Configuration grammar](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/animations.c#L237) · [Runtime toggle](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands.c#L167) · [Path fallback](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/animation.c#L334).

Retargeting is per animated variable. A destination unchanged within the variable's tolerance retains the earlier curve and remaining duration; a changed destination starts from the displayed value. Position changes use a one-pixel tolerance, independently of the smaller general tolerance. Fade progress is clamped to its normal range and animated sizes remain at least one layout unit. Removing the last participating output first tries other enabled outputs, then finishes animation state and releases a delayed transaction if no frame can be scheduled. [source] [Variable application](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/animation.c#L776) · [Retarget predicate](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/animation.c#L914) · [Output removal](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/animation.c#L513).

The floating restack detector compares consecutive shown layouts only within the same workspace. It finds overlapping floats whose relative depth changed, excluding tiled/fullscreen depths. Every placement sharing one floating depth contributes to that float’s union box, so grouped contents move together. Immediate repeated swaps have alternating animation names, allowing the CSS animation to restart. [source] [Restack scope](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/restacking.ts#L33) · [Overlap and depth](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/restacking.ts#L68) · [Animation names](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-styles.ts#L352).

Scroll’s scene-animation traversal includes retained unmapped views. It temporarily substitutes animated alpha for the pending alpha during that update, then restores the prior pending value. Its default animation step updates the scene root, while the separate end callback rebases cursors. Physical presentation and input delivery remain separately unqualified. [source] [Retained view update](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L1951) · [Step and end callbacks](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L2076).

The general curve path uses a 100-interval approximation to arc-length parameterization; simple cubic mode instead builds an x-to-y lookup. Sampling interpolates between lookup entries, and each curve carries four cached evaluations. Smoothness and physical frame timing remain unverified. [source] [Lookup construction](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/animation.c#L82) · [Arc-length approximation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/animation.c#L101) · [Sampling](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/animation.c#L679).

Each output step temporarily installs the animation output filter and sets the current output for the scene callback. It then clears that pointer and restores the previous filter. When the variable-count check reaches zero, one final callback runs without that current-output restriction before animation stop, output reset and delayed-transaction release. Other saved filters and physical presentation remain separate. [source] [Per-output and final step](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/animation.c#L641).

## Lineage

- 2026-03-27: a public performance discussion separated a shell toolkit's animation loop from Scroll's compositor animation loop. It did not establish a shared cause for every visible stall. [docs] [Public discussion](https://github.com/dawsers/scroll/issues/238#issuecomment-4141216561).

## At the pin

Not addressed at the pin: the donor decoration, border, gap or motion-curve knob is not an exposed Manganese command/configuration field; CSS rendering support is a separate existing primitive. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35) · [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L160) · [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L60).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-0e5ba5f15aa1 · 2026-11-05 · Scroll reference at the donor revision.
- S-12e11cee7f48 · 2026-11-05 · Scroll reference at the donor revision.
- S-1f455f8b0a46 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-26338d5a2c95 · 2026-11-05 · Scroll reference at the donor revision.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-4cf15b4011fd · 2026-03-27 · public animation-loop performance discussion.
- S-4f20a12f585a · 2026-11-08 · Niri reference at the donor revision.
- S-5d34e53f820b · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-70ac6562eb03 · 2026-11-08 · Niri reference at the donor revision.
- S-7f2d34d0bfe7 · 2026-11-05 · Scroll reference at the donor revision.
- S-9c59f8ebc0df · 2026-11-05 · Scroll reference at the donor revision.
- S-ab6f88da4d6c · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-b23d6954c82b · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-c0ee57a62724 · 2026-11-05 · Scroll reference at the donor revision.
- S-e77ffff40807 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
