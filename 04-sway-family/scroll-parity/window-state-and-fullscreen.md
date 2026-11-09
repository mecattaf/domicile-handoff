---
id: sway-family/scroll-parity/window-state-and-fullscreen
title: Window state and fullscreen
kind: family
family: layout
layers: [manganese, sdk, compositor]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-0d9c2ee30def, S-17c6354d2de5, S-1fa1acbff487, S-204a969601d0, S-23c521755f22, S-36bae2cd0f28, S-4a2b900f6a8d, S-6792066180dd, S-8c438f028a0f, S-9c59f8ebc0df, S-a5e842ad3658, S-a70560fdc087, S-ab155e5edf1e, S-d5b15decb594, S-e77ffff40807, S-f7e6c6503485]
---

# Window state and fullscreen

32 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 17 same; 5 scroll-changed; 10 scroll-only; 0 sway-only.
- Domicile coverage: 1 landed; 6 partial; 23 absent; 2 unverified; 0 accounting aliases/exclusions.
- Mechanism layers: compositor 5; manganese 32; sdk 32.
- Feature: [layout/tiling-splits-and-containers](../../06-features/layout/tiling-splits-and-containers.md).

## Donor semantics

Scroll separates application fullscreen UI from workspace/global container geometry. Layout fullscreen keeps a tiled item at fullscreen dimensions while permitting the workspace to scroll away from it. Configurable focus-follow behavior decides whether fullscreen moves with focus. [source] [Fullscreen dimensions](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L561) · [Focus relation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L218).

Manganese has screen and desktop-wide fullscreen, including selected groups. Native client states are sent independently through the SDK; the shell's composed geometry remains a separate concern. These mechanisms cover parts of the donor union without making the full Scroll layout/fullscreen policy present. [source] [Manganese fullscreen](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L138) · [SDK state action](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/windows.ts#L27).

At the pin, Manganese reacts to a client fullscreen request by filling its screen, while a maximize request leaves the layout unchanged. Its complete state and command inventories do not contain a scrolling-layout full-size item or a maximize-to-layout-dimensions policy. Ordinary screen/global fullscreen and independent SDK XDG-state flags remain separate mechanisms. [source] [Current request policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L138) · [Complete command forms](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35) · [Native-state interface](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L454).

Native transient-parent identity is readable separately from popup parent and follows parent changes. The complete Manganese native diff, window record and placement path do not retain or inspect that identity for parent-aware fullscreen placement. This shell-policy bound leaves the full engine/compositor ancestry and stacking contract unverified. [source] [Transient-parent identity](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1198) · [Shell record](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window.ts#L34) · [Native diff](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-changes.ts#L46) · [Placement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/placement.ts#L176).

The fullscreen command has explicit state guards. Layout fullscreen rejects floating or ordinary-fullscreen targets. While the layout-fullscreen flag is enabled, the command rejects ordinary and application-only variants. With no target it succeeds unchanged; with no connected output it fails. Scratchpad targets are promoted to their highest containing node before processing. [source] [Target and state guards](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/fullscreen.c#L51).

Client requests select a request policy separately from the command. Existing floating or ordinary-fullscreen state forces ordinary handling, while existing layout fullscreen retains layout handling. A client exit clears application fullscreen but preserves user-requested container geometry. The layout setter returns unchanged without a workspace or while workspace scaling is enabled, even though its command wrapper reports success. [source] [Request state machine](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/container.c#L1774) · [Layout setter](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/container.c#L1748) · [Command result](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/fullscreen.c#L104).

A managed native fullscreen request can move its window to the requested live display’s current workspace before fullscreening it. Exiting fullscreen leaves the window there. Missing, removed or already-current display destinations do not trigger that move. Native maximize and unmaximize requests produce no layout action in the current dispatch table. [source] [Request dispatch](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindows.ts#L230) · [Destination guard](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2341).

Scroll chooses scene layers for eligible floating transients using their relationship to a matching global fullscreen or visible virtual-region fullscreen. This can place those transients above the matching fullscreen content. Client state flags and the complete Domicile ancestry/stacking contract remain separate. [source] [Transient scene selection](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L1380) · [Global and region fullscreen cases](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L1393).

## At the pin

Partial: Manganese supports screen/global fullscreen and native client states; independent Scroll application/layout fullscreen policy or exact transient request behavior is a wider contract. [source] [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L138) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1139) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-17c6354d2de5 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-1fa1acbff487 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-36bae2cd0f28 · 2026-11-05 · Scroll reference at the donor revision.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-6792066180dd · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-9c59f8ebc0df · 2026-11-05 · Scroll reference at the donor revision.
- S-a5e842ad3658 · 2026-11-05 · Scroll reference at the donor revision.
- S-a70560fdc087 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-ab155e5edf1e · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-e77ffff40807 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-f7e6c6503485 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
