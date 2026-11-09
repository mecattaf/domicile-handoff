---
id: sway-family/scroll-parity/trails-and-advanced-marking
title: Trails, selections and saved spaces
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
sources: [S-0d9c2ee30def, S-204a969601d0, S-23c521755f22, S-3dc4b8b29e37, S-4a2b900f6a8d, S-7bafe2e8d4cd, S-8c438f028a0f, S-9bd7890dcb39, S-d5b15decb594, S-dae213c8d0b8, S-f612bacbe4ce]
---

# Trails, selections and saved spaces

50 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 0 same; 0 scroll-changed; 50 scroll-only; 0 sway-only.
- Domicile coverage: 0 landed; 0 partial; 50 absent; 0 unverified; 0 accounting aliases/exclusions.
- Mechanism layers: any-shell 50.
- Feature: [windows/marks-and-advanced-marking](../../06-features/windows/marks-and-advanced-marking.md).

## Donor semantics

Trails are ordered sets of anonymous window marks. Membership can overlap across trails. A trail has navigation, clearing, deletion and selection conversion; a selection can create a new active trail. [source] [Trails](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1109).

Selections span workspaces and preserve their ordering and grouping when moved. Destination mode controls insertion position, and an orientation change transposes the arrangement. These operations differ from selecting only an enclosing tree node. [source] [Selections](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L890).

A saved space records living windows and their configuration. Loading gathers surviving members into the current workspace without relaunching closed clients. Restore additionally invokes the close path for nonmembers in the destination; restore-hide moves those extras to the scratchpad. The source stores these records in memory, so this is distinct from durable session restoration. Unmapping clears the saved reference to that view. Floating geometry is stored as fractions of the source output and scales to the destination output; tiled geometry uses layout units. [source] [Spaces](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L992) · [Live references](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/space.c#L7) · [Floating geometry](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/space.c#L295).

Pins anchor one primary-axis group to a workspace edge. Repeating its current edge removes the pin; choosing the other edge moves it; pinning another group replaces it. Pins and marks are different state. [source] [Pin rules](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L773).

Trail numbering is zero-based. Choosing the next or previous trail wraps, and moving between marks wraps independently inside the active trail. Toggling a mark creates a trail if none exists. Selection-to-trail creates a new trail, gathers selected live views across outputs and clears their selection flags; it removes the new trail if no marks were gathered. Conversion back adds the active trail's members to the selection. Unmapping removes a view from every trail that contains it. [source] [Selection conversion](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3875) · [Trail state](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3947) · [Mark navigation and cleanup](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L4011).


Selection is stored as flags on groups or leaves. Toggle promotes a child to its parent when working mode matches workspace orientation. Move gathers members by output, workspace and existing container-list order, preserving encountered grouping rather than recording a separate selection-time sequence. Selected siblings extracted from a larger group receive a new enclosing group. Tiled groups enter at one index chosen by the destination insertion modifier; floating members stay floating. Selected top-level floating views translate by the source-to-destination workspace-origin difference. Destination creation-focus policy then controls focus. [source] [Selection flags](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3606) · [Group extraction](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3675) · [Collection and insertion](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3754).


For XDG clients, the restore close path sends the client a close request and continues reconstruction; it does not establish forced process termination or that every client has exited. Save replaces an existing record with the same name. Before dispatching a space operation, the parser disables active workspace fullscreen, including for save and delete. Missing-name load/delete does not rebuild a workspace, and the parser still reports success after its preliminary fullscreen handling. [source] [Restore order](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/space.c#L202) · [XDG close request](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/xdg_shell.c#L256) · [Name replacement and lookup](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/space.c#L365) · [Command dispatch](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/space.c#L25).

Reconstruction detaches surviving members, removes any scratchpad membership and restores each saved content factor before rebuilding tiled or floating placement. A missing saved focused member falls back to workspace focus. The saved record does not relaunch that client. [source] [Tiled reconstruction](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/space.c#L81) · [Floating reconstruction and focus fallback](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/space.c#L115).

## Lineage

- 2026-10-01: a public discussion distinguishes external durable templates and application matching from Scroll's live saved-space records and observation APIs. It describes an external scripting boundary, rather than an implemented general session-restoration contract. [docs] [Public discussion](https://github.com/dawsers/scroll/discussions/384#discussioncomment-18696436).

## At the pin

Not addressed at the pin: named search of the complete command table and state declarations for mark, trail, selection, jump and saved-space contracts returns no corresponding Manganese surface. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L121).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-3dc4b8b29e37 · 2026-11-05 · Scroll reference at the donor revision.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-7bafe2e8d4cd · 2026-11-05 · Scroll reference at the donor revision.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-9bd7890dcb39 · 2026-10-01 · public live-space and external-template discussion.
- S-dae213c8d0b8 · 2026-11-05 · Scroll reference at the donor revision.
- S-f612bacbe4ce · 2026-11-05 · Scroll reference at the donor revision.
