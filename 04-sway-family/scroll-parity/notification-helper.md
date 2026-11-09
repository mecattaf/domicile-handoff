---
id: sway-family/scroll-parity/notification-helper
title: Notification helper contracts
kind: family
family: bar-and-notify
layers: [manganese, system-apps]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-0d9c2ee30def, S-204a969601d0, S-23c521755f22, S-541270a86f10, S-d5b15decb594]
---

# Notification helper contracts

107 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 90 same; 4 scroll-changed; 7 scroll-only; 6 sway-only.
- Domicile coverage: 0 landed; 0 partial; 107 absent; 0 unverified; 0 accounting aliases/exclusions.
- Mechanism layers: manganese 107; system-apps 107.
- Feature: [bar-and-notify/notifications](../../06-features/bar-and-notify/notifications.md).

## Donor semantics

The donor notification helper presents command-driven warnings and button actions. Its CLI placement, sizing, spacing and config options are helper-process contracts, separate from a desktop notification service. Scroll adds its own helper placement controls. [source] [Scroll helper manual](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/swaynag/scrollnag.1.scd#L1).

Manganese exports a notification package and a drawer command. Those facts do not establish compatibility with the donor helper's CLI or configuration. The table records the latter separately from desktop notification support. [source] [Reusable packages](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md#L71) · [Notification command](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L76).

## At the pin

Not addressed at the pin: the donor notification-helper option/CLI contract is not in Manganese commands; its notification package is a different surface. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35) · [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md#L71).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-541270a86f10 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
