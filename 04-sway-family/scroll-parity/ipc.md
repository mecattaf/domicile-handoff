---
id: sway-family/scroll-parity/ipc
title: IPC observations and events
kind: family
family: ipc-scripting
layers: [sdk, compositor]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll, niri]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-10cfd46e0cfd, S-1fa1acbff487, S-204a969601d0, S-23c521755f22, S-278d2a39212b, S-2d260845be4d, S-311446be18e0, S-33784ae23b70, S-52d31a0991c7, S-8c438f028a0f, S-a5c59a9cf1ff, S-a70560fdc087, S-d5b15decb594, S-f278a9071404, S-fbe8a9d8422d]
---

# IPC observations and events

372 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 272 same; 0 scroll-changed; 100 scroll-only; 0 sway-only.
- Domicile coverage: 7 landed; 46 partial; 294 absent; 0 unverified; 25 accounting aliases/exclusions.
- Mechanism layers: compositor 372; sdk 372.
- Feature: [ipc-scripting/read-model-and-events](../../06-features/ipc-scripting/read-model-and-events.md).

## Donor semantics

Sway uses request/reply messages and subscriptions. Scroll retains that transport and adds scroller, trails, spaces, complete binding tables and Lua evaluation requests. Its tree/output/workspace replies also include content rectangles, visibility, sizing fractions and virtual-region state. [source] [Scroller](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-ipc.7.scd#L1495) · [Trails](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-ipc.7.scd#L1564) · [Spaces](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-ipc.7.scd#L1596).

Additional event groups report Lua messages, scroller-state changes and trail changes. A snapshot field, its change event and the registry entry naming that event remain separate accounting rows; aliases are not additional features. [source] [Lua events](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-ipc.7.scd#L2338) · [Scroller events](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-ipc.7.scd#L2353) · [Trail events](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-ipc.7.scd#L2399).

Domicile exposes typed namespaces with state attributes and listener registration. Ordinary window metadata and display geometry have individual crosswalk rows. A flat native-window list does not reproduce a compositor-owned workspace tree, and Manganese state does not make the donor wire protocol present. [source] [Window namespace](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/windows.ts#L18) · [Native fields](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1139).
The scroller change event contains a change label and the complete current scroller object, including workspace name, overview/scale and the layout modifiers. It is a snapshot with a change reason, rather than a field-only delta. [source] [Event construction](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/ipc-server.c#L470) · [Object serialization](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/ipc-json.c#L1572).

Window geometry also has separate frames of reference. The tree's content rectangle is relative to its node; its surface-content rectangle is the client-declared content box relative to the native surface. The latter can locate content inside a captured surface with client-drawn decoration. Domicile's client-frame property is a boolean decoration preference, not that rectangle. [source] [Donor fields](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-ipc.7.scd#L352) · [Current export](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/ipc-json.c#L623) · [Typed preference](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1190).
Niri's event stream supplies initial state followed by updates. Cross-object changes are not globally atomic: a window update can temporarily name a workspace already removed by an earlier event. This is another contract distinction from transport compatibility or the existence of a native-window list. [source] [Event-stream ordering](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/IPC.md#L11).

Native window identifiers are separate from shell container identity. The complete Manganese container type stores children, layout, focus and size fractions without its own identifier. Its internal structural reference uses a window identifier and ancestor count, validated against the current tree. This is not a public stable whole-tree node-ID API. [source] [Container fields](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/tree/node.ts#L28) · [Structural references](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/tree/path.ts#L13).

Current sandbox identity is populated, not only declared: the native appearance handler copies received owner metadata into the window state, and its DOM getters expose those values when supplied. Urgency has a narrower scope: a current event assigns one app's urgency bit, whose SDK meaning is a dropped keyboard request. That leaf record is distinct from the donor tree field aggregating a node and its descendants. [source] [Sandbox assignment](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1491) · [Readback getters](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_window.h#L74) · [Urgency assignment](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1634) · [SDK urgency meaning](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1211).

Display readback also has value-domain limits. The published transform contract lists four rotations and documents normal fallback for other names; it does not establish the donor reflected-transform domain. Scale readback names the density advertised to Wayland clients, separately from the shell page's density. The donor disabled-output sentinel is a different wire convention and remains unverified here. [source] [Transform names](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L367) · [Client scale](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L352).

## At the pin

Partial: typed SDK window/display state, change listeners and shell commands cover part of this observation/action contract; the donor wire request and complete payload are absent. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/windows.ts#L18) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/displays.ts#L21) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L121) · [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L38).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-10cfd46e0cfd · 2026-11-05 · Scroll reference at the donor revision.
- S-1fa1acbff487 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-278d2a39212b · 2026-11-05 · Scroll reference at the donor revision.
- S-2d260845be4d · 2026-11-05 · Scroll reference at the donor revision.
- S-311446be18e0 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-33784ae23b70 · 2026-11-08 · Niri reference at the donor revision.
- S-52d31a0991c7 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-a5c59a9cf1ff · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-a70560fdc087 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-f278a9071404 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-fbe8a9d8422d · 2026-11-09 · Domicile shell and typed-state reference at the pin.
