---
id: sway-family/scroll-parity/criteria-and-window-rules
title: Criteria and window rules
kind: family
family: windows
layers: [manganese, sdk]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll, niri]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-0d9c2ee30def, S-1712fb8a60e1, S-204a969601d0, S-23c521755f22, S-4a2b900f6a8d, S-6792066180dd, S-759448a412d0, S-8c438f028a0f, S-a70560fdc087, S-b1de89f2e134, S-d1e267110753, S-d5b15decb594]
---

# Criteria and window rules

50 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 50 same; 0 scroll-changed; 0 scroll-only; 0 sway-only.
- Domicile coverage: 0 landed; 4 partial; 46 absent; 0 unverified; 0 accounting aliases/exclusions.
- Mechanism layers: manganese 50; sdk 50.
- Feature: [windows/criteria-and-window-rules](../../06-features/windows/criteria-and-window-rules.md).

## Donor semantics

Criteria combine window attributes into a target set. The donor distinguishes application identity and title, container identity and marks, workspace membership, floating/tiling state, urgency, process identity, sandbox metadata and X11-only properties. Rules execute actions against matches, while assignment and no-focus rules add placement/focus behavior. [source] [Criteria](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1974) · [Rules](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1629).

Manganese supports a narrower first-draw rule: regular expressions on the client's application identity and title can open a window floating. Parented and fixed-size clients also open floating. The command guide's statement that general `for_window` is absent does not mean every client opens tiled. [source] [Floating decisions](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L204) · [Command differences](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L160).

The row map keeps a predicate's readable native field separate from a general criteria engine and from repeated rule evaluation after metadata changes. [source] [Native fields](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1139).
Niri merges matching rules in configuration order. Alternatives within the rule are OR, fields inside one matcher are AND, and exclusions veto a match. Opening properties apply once, while later state can affect continuous properties. In particular, floating-state matching starts after opening and cannot choose opening placement or size. [source] [Rule ordering](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Configuration:-Window-Rules.md#L3) · [Matcher logic](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Configuration:-Window-Rules.md#L133) · [Floating-state timing](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Configuration:-Window-Rules.md#L255).

Current source evaluates floating app/title rules only when a native toplevel first appears. Later title and application-identity diffs update the window record without reevaluating that opening decision. This distinguishes first-appearance matching from the donor's later opportunity to match an additional rule. [source] [Opening match](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/floating/floating-rules.ts#L37) · [Appearance versus metadata diff](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-changes.ts#L46) · [Record update](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1311).

Sway’s command-target collector distinguishes view-backed and non-view containers. View-backed targets use the view matcher; a non-view branch can apply container predicates when con_mark or con_id is present. That command selection differs from per-view rule evaluation. The literal mark finder and regular-expression con_mark predicate are separate lookups. [source] [Command target collector](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/criteria.c#L500) · [Rule matcher](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/criteria.c#L483) · [Literal traversal](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/root.c#L292).

## At the pin

Not addressed at the pin: there is no general criteria/assign/no_focus command surface in the complete Manganese command table; first-draw floating rules are narrower. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35) · [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L204).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-1712fb8a60e1 · 2026-11-08 · Niri reference at the donor revision.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-6792066180dd · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-759448a412d0 · 2026-09-21 · sway reference at the donor revision.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-a70560fdc087 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-b1de89f2e134 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-d1e267110753 · 2026-09-21 · sway reference at the donor revision.
