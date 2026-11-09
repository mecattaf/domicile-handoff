---
id: bar-and-notify/status-bar-and-tray
title: Status bar modules and application tray
kind: feature
family: bar-and-notify
layers: [manganese, host, sdk, compositor, system-apps]
domicile_status: landed
domicile_refs: [cprussin/domicile#1855, docs/architecture/SYSTEM-TRAY.md, packages/shell-manganese/docs/TOP-BAR.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll]
eras: []
rows: [SW-0001, SW-0002, SW-0005, SW-0006, SW-0007, SW-0008, SW-0009, SW-0011, SW-0012, SW-0013, SW-0014, SW-0015, SW-0016, SW-0017, SW-0018, SW-0019, SW-0020, SW-0021, SW-0024, SW-0025, SW-0026, SW-0028, SW-0029, SW-0030, SW-0031, SW-0032, SW-0033, SW-0034, SW-0035, SW-0036, SW-0037, SW-0038, SW-0039, SW-0040, SW-0041, SW-0042, SW-0043, SW-0271, SC-0002, SC-0003, SC-0004, SW-0351, SW-0352, SW-0357, SW-0358, SW-0926, SW-0927, SW-0928, SW-0929, SW-0930, SW-0931, SW-0932, SW-0933, SW-0934, SW-0935, SW-0936, SW-0937, SW-0938, SW-0939, SW-0940, SW-0941, SW-0942, SW-0943, SW-0944, SW-0945, SW-0946, SW-0947, SW-0948, SW-0949, SW-0950, SW-0951, SW-0952, SW-0953, SW-0954, SW-0955, SW-0956, SW-1275, SW-1276, SW-1277, SW-1278, SW-1279, SW-1280, SW-1281, SW-1282, SW-1283, SW-1284, SW-1285, SW-1286, SW-1287, SW-1288, SW-1289, SW-1290]
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: [L-26]
boundary_note: false
bucket: domicile
tags: [family:bar-and-notify, layer:manganese, status:landed, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-0860f68cdd6d, S-0fb04aa4b182, S-179ac96a4030, S-1a653077d862, S-32b5544fb772, S-342981352c2a, S-484101b475e9, S-541270a86f10, S-71dcb9b929c5, S-9bade2fc7ff6, S-a6a7fef88d3a, S-e7980875b42d, S-ef762f2a87b5, S-f910672ea3a4, S-fd16ec144654]
---
# Status bar modules and application tray

The status bar combines independent desktop modules, while application tray items carry their own actions and menus.

## Behavior

- Modules expose workspace, launcher, clock, input mode and system state independently.
- Tray primary, secondary and context activation have separate meanings.
- An item marked as a menu opens its menu on primary activation.
- Menus preserve submenu navigation and application-owned actions.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| sway @ pinned source | swaybar configuration and IPC | Bar state is a dedicated IPC request family. | [Bar request](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-ipc.7.scd#L60) |
| scroll @ pinned source | Inherited bar-facing state with extra layout state | The inherited bar contract remains separate from the scrolling tree. | [Pinned donor](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa) |

## What the notes found

- Checked 2026-11-09: The shell parity comparison separates service capabilities from the visual composition of a bar. [docs]
- Checked 2026-11-09: Manganese tray icons send primary and middle activation separately. A secondary click, or primary click on ItemIsMenu, opens dbusmenu when supplied and otherwise requests the application’s own context menu. [source] [Tray action contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-bar-module-tray/src/TrayIcon.tsx#L52)
- Checked 2026-11-09: Manganese displays application-reported native menu toggle state. Clicks send a dbusmenu Event; submenu opening sends AboutToShow and refreshes the menu when the application reports a change. [source] [Tray action contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-bar-module-tray/src/TrayMenu.tsx#L130) · [Tray action contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/dbusmenu.ts#L126)
- 2026-11-09: Ash’s `TrayNetworkStateModel` binds a CrosNetworkConfig remote and observer receiver; its direct observer callbacks are a separate boundary from its Mojo service channel (ash@domicile-engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/system/network/tray_network_state_model.cc#L53>
- 2026-11-05: sway-bar separates hidden_state (hide or show) from mode (dock, hide, invisible, overlay), with a runtime-only mode toggle; overlay stays on top and passes input through. [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-bar.5.scd#L55-L75>
- 2026-10-28: The June shell reconstruction records a workspace sidebar whose collapse changed the native surface width and exclusive zone, allowing the external compositor to reclaim space (COWL@149) [docs]
- 2026-06-06: The June layer-shell plan distinguishes rendering a panel from reserving space: anchor, margin and exclusive-zone requests need compositor-owned per-output work-area and stacking behavior (cade@June-2026) [docs]
- 2026-06-29: The June dock draft identifies separate running-window and application-pin producers; it records unresolved sibling-widget dependencies and competing work-area writers, so the drafted shelf reuse is not a completed implementation (ash@149.0.7827.102) [docs]
- 2026-11-04: The donor audit separates persistent launcher provider lifetime from panel lifetime; provider instances can survive launcher recreation and are destroyed on deregistration (noctalia@2026-11-survey) [docs]

## Where it touches Domicile

- `manganese`: workspace, tray, launcher, clock, keymap, mode, hardware and notifications are exported as independent bar items. [source] [Module list](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/shell-manganese/docs/TOP-BAR.md#L13)
- `compositor`: the tray D-Bus frontend supplies item updates and receives calls. [source] [D-Bus frontend contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/tray.rs#L1)
- `host`: the host library keeps the item registry, resolves shell names to bus/path addresses and maps activation requests to methods. [source] [Tray registry](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/tray.rs#L99) · [Activation mapping](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/tray.rs#L203)
- `sdk`: the desktop tray namespace exposes item snapshots and activation; applications implement their click actions and menus. [source] [Tray namespace](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-DESKTOP-EVENTS.md#L89)
- `system-apps`: launcher and displayed application identities use desktop entries. [source] [System apps layer](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ARCHITECTURE.md#L147)
- Status: `landed`; the tray and reusable bar modules are documented at the pin, alongside merged bar work in [#1855](https://github.com/cprussin/domicile/pull/1855). [source] [Current bar](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/shell-manganese/docs/TOP-BAR.md#L8)

## Lineage

- 2026-11-09: the pin exposes the bar and its modules as independently reusable packages. [source] [Take its parts](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md#L71)

## Sources

- S-32b5544fb772 · 2026-09-21 · [Desktop request and event protocol](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-ipc.7.scd#L1).
- S-179ac96a4030 · 2026-11-09 · [Modular bar controls and state producers](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/shell-manganese/docs/TOP-BAR.md#L1).
- S-a6a7fef88d3a · 2026-11-09 · [Typed desktop event namespaces](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-DESKTOP-EVENTS.md#L1).
- S-1a653077d862 · 2026-11-09 · [Shell, SDK and system-service boundaries](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ARCHITECTURE.md#L1).
- S-541270a86f10 · 2026-11-09 · [Reusable shell modules and public exports](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md#L1).
- S-0fb04aa4b182 · 2026-11-09 · [Native tray activation routing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-bar-module-tray/src/TrayIcon.tsx#L1).
- S-ef762f2a87b5 · 2026-11-09 · [Native menu toggles and application-owned action state](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-bar-module-tray/src/TrayMenu.tsx#L1).
- S-0860f68cdd6d · 2026-11-09 · [Native menu subscriptions, actions and refresh](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/dbusmenu.ts#L1).
- S-f910672ea3a4 · 2026-11-09 · [Native tray registry and request mapping](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/tray.rs#L1).
