---
id: sway-family/scroll-parity/bar-and-status
title: Bar, status and tray
kind: family
family: bar-and-notify
layers: [manganese, sdk]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-1a2a546c8c28, S-204a969601d0, S-23c521755f22, S-243fffa7f89d, S-541270a86f10, S-d5b15decb594, S-d7ced2b3ded1]
---

# Bar, status and tray

92 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 89 same; 0 scroll-changed; 3 scroll-only; 0 sway-only.
- Domicile coverage: 0 landed; 5 partial; 86 absent; 0 unverified; 1 accounting aliases/exclusions.
- Mechanism layers: manganese 92; sdk 92.
- Feature: [bar-and-notify/status-bar-and-tray](../../06-features/bar-and-notify/status-bar-and-tray.md).

## Donor semantics

Swaybar and Scrollbar have their own configuration, status-block protocol and click-event contracts. Bar identity, visibility, output assignment and binding-mode display differ from the window manager's own command language. Scroll adds scroller and trail indicators. [source] [Scroll bar manual](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-bar.5.scd#L1).

Domicile's published Manganese surface uses React components for bar items, reads native state through hooks and exports the bar and items as packages. That provides reusable bar machinery while leaving donor bar-command and status-protocol compatibility separate. [source] [Bar options](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md#L32) · [Custom item](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md#L46).

Mode indication is available for non-default modes, but omitted by the normal defaults. Workspace controls are present by default; the supplied control also carries the overview action. Desktop-bar configuration does not alter the separate lock bar. These scopes are recorded as partial counterparts to independent donor bar flags. [source] [Mode behavior](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-bar-module-mode/src/bar-item.tsx#L4) · [Workspace-control behavior](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-bar-module-workspace-switcher/src/bar-item.tsx#L14) · [Default scopes](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/shell-manganese/src/default-bars.tsx#L27).

## At the pin

Not addressed at the pin: swaybar/scrollbar configuration and status-protocol members are not exposed by Manganese; its bar uses React package options and components. [docs] [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md#L32) · [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md#L46) · [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L38).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-1a2a546c8c28 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-243fffa7f89d · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-541270a86f10 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-d7ced2b3ded1 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
