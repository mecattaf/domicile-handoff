---
id: sway-family/scroll-parity/tiling-splits-and-containers
title: Tiling splits and containers
kind: family
family: layout
layers: [manganese]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll, niri]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-0d7150dbb6e9, S-0d9c2ee30def, S-102d2919cad6, S-204a969601d0, S-23c521755f22, S-d5b15decb594]
---

# Tiling splits and containers

31 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 0 same; 2 scroll-changed; 0 scroll-only; 29 sway-only.
- Domicile coverage: 0 landed; 11 partial; 20 absent; 0 unverified; 0 accounting aliases/exclusions.
- Mechanism layers: manganese 31.
- Feature: [layout/tiling-splits-and-containers](../../06-features/layout/tiling-splits-and-containers.md).

## Donor semantics

Sway's layout tree allows split, tabbed and stacked containers, nested selection and layout changes. Scroll replaces those original layout choices with its two-level scrolling organization; a common command spelling can therefore denote different spatial behavior. [source] [Donor comparison](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/README.md#what-are-the-differences-between-sway-and-scroll).

Manganese implements nested split/tabbed/stacking groups. Every tiled window lives in a tab group. A new window enters as a tab; a split action moves an existing tab beside or below its group. A selected group can move or float as a unit. These differences are observable insertion/selection rules, rather than presentation details. [source] [Current tree behavior](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L6) · [Nested groups](https://github.com/cprussin/domicile/pull/1880).
Niri offers normal vertical tiles or tabbed display within a column. Changing display preserves column operations; a tabbed column can fullscreen multiple windows, while a regular column cannot. This is a column-level contract, separate from sway's arbitrary container tree and Manganese's nested groups. [source] [Tabbed columns](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Tabs.md#L5).

## At the pin

Not addressed at the pin: this exact layout choice or split option is not exposed by the complete command parser; related nested groups and layout toggles are present. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35) · [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L6).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d7150dbb6e9 · 2026-11-08 · Niri reference at the donor revision.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-102d2919cad6 · 2026-11-05 · Scroll reference at the donor revision.
