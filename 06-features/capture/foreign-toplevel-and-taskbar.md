---
id: capture/foreign-toplevel-and-taskbar
title: Foreign toplevel lists and window management
kind: feature
family: capture
layers: [compositor, protocol, sdk, any-shell]
domicile_status: landed-with-gap
domicile_refs: [cprussin/domicile#1510]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, exo]
rows: [ext-foreign-toplevel-list-v1, wlr-foreign-toplevel-management-v1]
evidence_mix: mostly-source
boundary_note: true
bucket: domicile
tags: [family:capture, status:landed-with-gap, evidence:source, scroll-parity:baseline]
sources: [S-3f524db52413, S-43af2b3af183, S-5a87c290061e, S-6ca6958d79ba, S-866b9f3f829d, S-8a46216d2a3e, S-c2596d00b100, S-c5d3db1884f1, S-e7392f226f69]
---
# Foreign toplevel lists and window management

Foreign-toplevel protocols let a client enumerate native Wayland windows and, where a management protocol permits, request window actions. Domicile also exposes native and browser state directly to the shell. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L501) [docs]

## Behavior

- The ext foreign-toplevel list feeds native-window image-capture sources. Browser windows are omitted from this Wayland list. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L506) [docs]
- A list protocol, a management protocol and xdg foreign parent/export relationships have different purposes. [docs]

- Domicile advertises the read-only ext list. The wlr foreign-toplevel management global is absent, so a taskbar cannot bind that management contract. Native-window state changes and closing remain available to the shell through its SDK request path. [source] [List registration](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/foreign_toplevels.rs#L28) · [Native-window state request](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/client_requests.rs#L588) · [Close request](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/client_requests.rs#L640)

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Foreign-toplevel management and ext toplevel list | Taskbar actions and capture-source enumeration have separate interfaces | [pinned server](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/server.c#L473) |
| scroll | Carries sway foreign-toplevel and capture integration | Window layout commands remain compositor/shell policy | [pinned server](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/server.c#L476) |

## What the notes found

- Window enumeration is distinct from window management. A toplevel handle is also a capture-source identifier. [inferred]
- 2026-10-02: A 2026-10-02 shell study identifies the window-identity join as independent work: compositor IPC node identifiers do not automatically equal ext-foreign-toplevel-list identifiers (views@2026-10-02) [docs]
- 2026-11-08: Browser windows missing from the window list and from capture is a known gap in Domicile's own docs, per a 2026-11-08 parity study [docs]

## Where it touches Domicile

- `compositor` serves ext foreign-toplevel list beside the direct capture protocols. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L501) [docs]
- Status at the pin: landed-with-gap, following [#1510](https://github.com/cprussin/domicile/pull/1510). The direct list covers Wayland windows; browser windows are outside that list. The SDK supplies browser-window state to the shell through another route. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L506) [docs]

## Lineage

The protocol history is indexed by collection.

Typical clients: screenshot and recording tools, remote desktop, clipboard managers, accessibility and test automation.

## Sources

- S-c5d3db1884f1 · 2026-10-24 · Foreign-toplevel and native-window capture merge record.

- S-5a87c290061e · 2026-11-09 · Native-window control requests.

- S-8a46216d2a3e · 2026-11-09 · Read-only foreign-toplevel list registration.

- S-43af2b3af183 · 2026-11-09 · Pinned donor implementation locator.

- S-e7392f226f69 · 2026-11-09 · Pinned donor implementation locator.

- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-c2596d00b100 · 2026-11-09 · Portal interfaces, direct capture and sandbox boundary.
