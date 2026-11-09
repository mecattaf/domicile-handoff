---
id: sway-family/scroll-parity/jump
title: Jump labels
kind: family
family: windows
layers: [any-shell]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-0d9c2ee30def, S-204a969601d0, S-23c521755f22, S-4a2b900f6a8d, S-83fe9a2cb7f8, S-8c438f028a0f, S-d5b15decb594, S-dae213c8d0b8]
---

# Jump labels

26 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 0 same; 3 scroll-changed; 23 scroll-only; 0 sway-only.
- Domicile coverage: 0 landed; 0 partial; 24 absent; 0 unverified; 2 accounting aliases/exclusions.
- Mechanism layers: any-shell 26.
- Feature: [windows/marks-and-advanced-marking](../../06-features/windows/marks-and-advanced-marking.md).

## Donor semantics

Jump draws key sequences over a chosen window set. Completing a sequence selects its target; clicking selects directly; an unrelated key exits while preserving the prior focus. The set can be a container, tiled windows, floats, trail members, visible windows or a criteria match. A workspace variant labels workspaces instead. Floating previews are rearranged so that covered windows remain selectable. [source] [Jump](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L596).

Label text, matching key symbols and consumed-prefix display are independently configurable. That separation permits labels whose printed characters differ from the keyboard layout's key symbols. [source] [Label controls](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L262).

The parser separates target kind from workspace scope: the first `all` selects tiled plus floating targets, while a second `all` selects all workspaces. The implementation passes that independently parsed scope to the target routine. Criteria jump accepts its own two-argument form. [source] [Parser and dispatch](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/jump.c#L97).

Global fullscreen and the absence of outputs reject the command. Criteria selection has its own grammar and searches across workspaces. A request made during an existing jump leaves that jump in place; an empty target set restores the earlier workspace overview and filters without installing the modal input override. Only key presses advance a label sequence. An unrelated key or an unused complete sequence exits without selecting a new target. [source] [Command guards and criteria](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/jump.c#L84) · [Target setup](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L2838) · [Key handling](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L2781).

Window-jump completion restores saved floating positions and overview state, resolves the selected or earlier focus by live container ID, and clears the jumping flag. The Lua jump-end callback then runs before the dirty transaction commit and removal of the input override. [source] [Restoration](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L2692) · [Completion order](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L2740).

Default jump selects the layout visible filter. That filter tests container-bound containment and does not call the separate full-visibility query’s floating-overlap test. Its documented completely-visible wording therefore has this source-level scope. [source] [Default jump](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L2914) · [Filter implementation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L1854).

The temporary jump arrangement starts from saved positions on each recomputation. Floating overlap is resolved by selecting a maximum-overlap candidate in node-ID order and moving it along the smaller displacement axis; included content is then re-centered. This arrangement is separate from the ordinary viewport order. [source] [Deterministic candidate order](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L2271) · [Overlap displacement](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L2321) · [Recomputation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L2421).

Workspace jump has a separate completion path that restores the earlier global-overview state. Its pointer picker includes both rectangle edges and retains the last matching workspace in output/workspace iteration order; this differs from the half-open mapper used by ordinary global overview. Cancellation resolves the earlier focus by its live identifier. [source] [Workspace completion](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3009) · [Inclusive picker](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3039).

## At the pin

Not addressed at the pin: named search of the complete command table and state declarations for mark, trail, selection, jump and saved-space contracts returns no corresponding Manganese surface. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L121).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-83fe9a2cb7f8 · 2026-11-05 · Scroll reference at the donor revision.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-dae213c8d0b8 · 2026-11-05 · Scroll reference at the donor revision.
