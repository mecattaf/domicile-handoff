---
id: sway-family/scroll-parity/marks
title: Marks
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
sources: [S-017f66cda9c3, S-0d9c2ee30def, S-16174e406a0b, S-204a969601d0, S-23c521755f22, S-4a2b900f6a8d, S-759448a412d0, S-8c438f028a0f, S-d1e267110753, S-d5b15decb594]
---

# Marks

19 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 19 same; 0 scroll-changed; 0 scroll-only; 0 sway-only.
- Domicile coverage: 0 landed; 0 partial; 16 absent; 0 unverified; 3 accounting aliases/exclusions.
- Mechanism layers: any-shell 19.
- Feature: [windows/marks-and-advanced-marking](../../06-features/windows/marks-and-advanced-marking.md).

## Donor semantics

Named marks attach addressable identifiers to containers. Add preserves other marks, replace removes earlier marks and toggle removes an existing matching mark. Marks participate in criteria, focus, movement and swapping; removing a named mark differs from clearing all marks. [source] [Marks](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1677) · [Unmark](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1778).

A mark's identity is separate from its displayed label and from the anonymous membership used by Scroll trails. Trail conversion, saved spaces and jump labels are indexed in their own families. [source] [Trails](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1109) · [Jump](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L596).

At the Sway pin, marks label containers including groups without a view. Assigning an identifier removes that literal identifier from its previous container. Replace clears the target’s existing marks, add preserves them, and toggle can remove the identifier. Literal lookup uses equality and traverses output containers, hidden scratchpad trees and saved fallback workspaces; con_mark criteria instead use regular expressions. [source] [Container-capable command](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/mark.c#L20) · [Storage and literal identity](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/container.c#L1635) · [Lookup traversal](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/root.c#L246) · [Criteria matching](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/criteria.c#L168).

## At the pin

Not addressed at the pin: named search of the complete command table and state declarations for mark, trail, selection, jump and saved-space contracts returns no corresponding Manganese surface. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L121).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-017f66cda9c3 · 2026-09-21 · sway reference at the donor revision.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-16174e406a0b · 2026-09-21 · sway reference at the donor revision.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-759448a412d0 · 2026-09-21 · sway reference at the donor revision.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-d1e267110753 · 2026-09-21 · sway reference at the donor revision.
