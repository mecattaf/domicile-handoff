---
id: sway-family/scroll-parity/workspaces-and-outputs
title: Workspaces and output rules
kind: family
family: workspaces
layers: [engine:drm, config, compositor, engine:viz, any-shell, engine:blink, manganese, protocol, sdk, external]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll, niri]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-0d9c2ee30def, S-204a969601d0, S-23c521755f22, S-2c05e7f10be8, S-3d1446ae145e, S-3f17426ddbf7, S-48603351a6ee, S-4a2b900f6a8d, S-515bf507459b, S-6b79c52fc5cd, S-74f557738f56, S-8100b35ed50a, S-8c438f028a0f, S-9c59f8ebc0df, S-d5b15decb594, S-f278a9071404]
---

# Workspaces and output rules

134 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 121 same; 5 scroll-changed; 8 scroll-only; 0 sway-only.
- Domicile coverage: 6 landed; 9 partial; 92 absent; 24 unverified; 3 accounting aliases/exclusions.
- Mechanism layers: any-shell 2; compositor 14; config 127; engine:blink 2; engine:drm 28; engine:viz 6; external 3; manganese 92; protocol 4; sdk 1.
- Feature: [workspaces/workspace-model-and-homes](../../06-features/workspaces/workspace-model-and-homes.md).

## Donor semantics

Sway and Scroll distinguish workspace names, numeric ordering, creation-time output preferences, current home, selection and moving a workspace. Ordered output preferences survive availability changes; a creation-time default does not itself move an existing workspace. Scroll additionally treats virtual regions as output-local workspace domains and can exchange workspace contents or only their names. [source] [Workspace rules](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1806) · [Output preferences](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1843) · [Swap](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1919).

Manganese has ten workspace names for the whole desktop. A workspace appears on at most one screen. A hidden populated workspace keeps its home while that screen survives; if its home disappears, it is reassigned to the focused screen. Selecting a workspace already visible elsewhere moves keyboard focus there. Its overview swaps contents and homes, while screen configuration belongs to the compositor config. [source] [Screens and homes](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L45) · [Overview swaps](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L369).

Physical output configuration, connectorless monitors and subdivisions of one output are separate row groups. [source] [Display namespace](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/displays.ts#L21).

## Desk terminology

An Ash desk spans displays and there is one active desk. That differs from selecting independent workspace homes on individual outputs. Desk activation also selects its most recently used window; add, remove, naming, reorder and activation are distinct observable changes. [source] [Desk scope](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.h#L29) · [Desk operations](https://github.com/chromium/chromium/blob/150.0.7871.124/ash/wm/desks/desks_controller.h#L76).
Niri uses a dynamic vertical workspace list on each monitor. A numeric index names the workspace currently at that position, rather than a stable workspace identity. Disconnected-monitor workspaces remember their output, but an ordinary workspace forgets that association when a new window opens there; a named workspace retains it until explicitly moved. This differs from Manganese's fixed workspace numbers and home assignment. [source] [Workspace lifetime](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Workspaces.md#L3) · [Monitor return policy](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Workspaces.md#L25) · [Index meaning](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Workspaces.md#L34).

The source implements per-screen next/previous stepping over the workspace home list with modulo wrapping. The merged gesture bindings invoke this existing helper. That evidence establishes current implementation and merged use, without assigning the integration's date as the helper's introduction. [source] [Step commands](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L99) · [Wrapping reducer](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2250) · [Merged gesture integration](https://github.com/cprussin/domicile/pull/1929).

Donor output policy includes fallback behavior: smart filtering selects nearest at integer scales and linear at fractional scales, while downscaling remains bilinear. Subpixel state restores detected order without an override. Adaptive-sync requests require reported support and retry disabled on state failure; tearing is limited to eligible fullscreen views and retries the frame without tearing on test failure. Background configuration launches a per-output background client. These source mechanisms do not establish their complete Domicile configuration or hardware equivalents. [source] [Filtering](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L650) · [Filtering](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L744) · [Filtering](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L193) · [Filtering](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L209) · [Subpixel state](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L507) · [Subpixel state](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L588) · [Adaptive-sync fallback](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L536) · [Adaptive-sync fallback](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L834) · [Adaptive-sync fallback](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L630) · [Adaptive-sync fallback](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L941) · [Tearing fallback](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L253) · [Tearing fallback](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L296) · [Tearing fallback](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L269) · [Tearing fallback](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L314) · [Background dispatch](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L1258) · [Background dispatch](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L1373).

Without an explicit render depth, donor HDR requests default to ten bits and other requests to eight. HDR descriptions require reported BT2020/PQ support and renderer color transforms, and an explicit color profile disables HDR. Hardware acceptance and physical-link depth remain separate. [source] [Render depth](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L464) · [Render depth](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L520) · [Render depth](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L898) · [HDR conditions](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L351) · [HDR conditions](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L557) · [HDR conditions](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L407) · [HDR conditions](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L651).

The current display conversion has a conditional BGRA8888 fallback when a connector snapshot lacks XBGR8888. That branch is format selection; exposed bit-depth control, HDR output and physical-link qualification remain unverified by it. [source] [Output format fallback](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/ozone/platform/drm/domicile/drm_screen.cc#L82).

On a display-list change, surviving screens retain their shown workspaces. New screens inherit the removed screens’ shown workspaces before taking unused names. This shown-workspace reassignment is separate from the fallback for a hidden populated workspace whose home disappeared. [source] [Shown workspace inheritance](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2170) · [Hidden home fallback](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2546).

The complete top-level output type declares described displays, maximum nested scale and physical-monitor profiles. Described displays advertise a nonzero integer scale; physical profiles accept fractional scale and round it upward for wl_output. Profiles can enable connectors and are re-matched on reload, but their mode field checks dimensions rather than choosing a mode or refresh rate. These positive layout contracts do not settle exact donor filtering, subpixel, background-client, adaptive-sync, tearing or HDR behavior. [source] [Output fields](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/lib.rs#L376) · [Described display scale](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/lib.rs#L264) · [Profile limits](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L50) · [Reload](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L24).

The donor view predicate has three states. An explicit false override denies tearing, a true override permits it, and hint mode permits it only when the client advertises asynchronous presentation. New views start in hint mode. Output-level permission, fullscreen eligibility and fallback after an output-state test remain separate gates. [source] [Sway view predicate](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/view.c#L1290) · [Sway initial mode](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/view.c#L110) · [Scroll view predicate](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1510) · [Scroll initial mode](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L113).

Scroll pans its tiled and fullscreen workspace layers, while floating containers receive individual offsets. Sticky floats receive no workspace pan. Multi-workspace overview restricts floating scene nodes to their workspace output. A queued inactive workspace can be arranged before it is disabled; the animation path hides inactive workspaces rather than advancing their normal workspace animation. Physical presentation timing remains unverified. [source] [Layer pan and floats](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L1358) · [Overview output restriction](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L1453) · [Inactive workspace updates](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L1759).

Sending the current selection to a different workspace tiles the selected window or group and keeps focus behind. Relocating a named window also tiles it, while preserving which workspace each screen shows. Workspace swapping exchanges contents and homes while retaining the fixed workspace names; shown-workspace names and the previous-workspace identifier are rewritten to follow the exchanged contents. [source] [Selected-group send](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2426) · [Named-window relocation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2447) · [Swap bookkeeping](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1236).

## At the pin

Not addressed at the pin: a profile mode asserts the reported pixel size and does not select a physical mode/refresh rate or create a custom modeline. [docs] [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L58) · [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md#L35).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-2c05e7f10be8 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-3d1446ae145e · 2026-11-05 · Scroll reference at the donor revision.
- S-3f17426ddbf7 · 2026-09-21 · sway reference at the donor revision.
- S-48603351a6ee · 2026-11-08 · Niri reference at the donor revision.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-515bf507459b · 2026-09-21 · sway reference at the donor revision.
- S-6b79c52fc5cd · 2026-11-05 · Scroll reference at the donor revision.
- S-74f557738f56 · 2026-11-09 · donor source accounting and current-state evidence.
- S-8100b35ed50a · 2026-11-09 · donor source accounting and current-state evidence.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-9c59f8ebc0df · 2026-11-05 · Scroll reference at the donor revision.
- S-f278a9071404 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
