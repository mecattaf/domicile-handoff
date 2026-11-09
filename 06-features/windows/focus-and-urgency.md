---
id: windows/focus-and-urgency
title: Pointer focus and activation policy
kind: feature
family: windows
layers: [manganese, sdk, compositor, engine:blink]
domicile_status: landed
domicile_refs: [cprussin/domicile#1892, packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, mutter, hyprland]
eras: []
rows: [SW-0134, SW-0135, SW-0136, SW-0137, SW-0138, SW-0139, SW-0140, SW-0142, SW-0207, SW-0208, SW-0209, SW-0226, SW-0242, SC-0032, SC-0033, SC-0069, SC-0070, SC-0103, SC-0104, SC-0116, SW-0438, SW-0439, SW-0440, SW-0441, SW-0442, SW-0443, SW-0444, SW-0445, SW-0446, SW-0447, SW-0585, SW-0586, SW-0587, SW-0588, SW-0589, SW-0590, SW-0591, SW-0592, SW-0593, SW-0594, SW-0595, SW-0625, SW-0626, SW-0627, SW-0649, SW-0650, SW-0651, SW-0652, SC-0252, SC-0253, SC-0254, SC-0255, SC-0256, SC-0257, SC-0258, SC-0259, SC-0260, SC-0261, SC-0262, SC-0263, SC-0348, SC-0349, SC-0350, SC-0351, SC-0352, SC-0353, SC-0354, SC-0355, SC-0356, SW-1027, SC-0506, SC-0532, SC-0533, SW-1081, SC-0612, SC-0613, SC-0616, SW-1144]
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: false
bucket: domicile
tags: [family:windows, layer:manganese, status:landed, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-069533fa6b43, S-14b41dc319ce, S-17c6354d2de5, S-17e38c60f482, S-1c58e1373c0b, S-1fa1acbff487, S-204a969601d0, S-23c521755f22, S-543465ec2563, S-7db6c4abc1a7, S-87a48f59dd5d, S-8c438f028a0f, S-8e001f804b21, S-900dfbb65723, S-9fe5a8a3f191, S-a4a9ba39e993, S-a89a4c46b8f9, S-bc2a6582ae19, S-bde2848501a4, S-bf5c4cf3a78c, S-c40e302fa0da, S-e1838c76399f, S-e77ffff40807, S-ecfec2f17332, S-fbe8a9d8422d, S-fd0c87388c02]
---
# Pointer focus and activation policy

Focus policy decides which window receives keys after pointer movement, directional navigation, closing and activation requests.

## Behavior

- Hover focus and raising are separate actions.
- Directional focus can move through a tree and across screens.
- Activation requests reveal the correct workspace and tab only when the request is accepted.
- Urgency and focus-on-activation are separate donor policy choices.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| sway @ pinned source | Pointer-focus and activation policy commands | Activation supports smart, urgent, focus and none; the default is urgency. | [Focus policies](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L731) |
| scroll @ pinned source | Additional viewport-aware pointer focus | The full pointer policy focuses tiled windows only when fully visible; activation defaults to focus. | [Focus policies](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1565) |
| Mutter @ pinned source | Activation timestamp policy and attention fallback | Stale timestamps and some untimed cross-workspace requests take the attention path. | [Activation policy](https://github.com/GNOME/mutter/blob/b766717ac9ad04d44211557ec79a545dc8c44ee7/src/core/window.c#L3974) |
| Hyprland @ pinned source | Soft, click and hard scrolling focus | Visibility and pointer-overlap guards distinguish focus sources; a fully visible column can retain the camera position. | [Focus guards](https://github.com/hyprwm/Hyprland/blob/151b0434114fad482e4fed9438c081b46e73423c/src/layout/algorithm/tiled/scrolling/ScrollingAlgorithm.cpp#L682) |

## What the notes found

- Checked 2026-11-09: The donor defaults differ even where command grammars match: sway defaults activation to urgency and Scroll defaults it to focus. [source] [sway activation](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L736) · [Scroll activation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1572)
- Checked 2026-11-09: Manganese ends initial compositor-focus replay after its first recognized report. Later reports update stored native focus independently of shell selection. [source] [Focus replay](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1391) · [Focus replay](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1959)
- Checked 2026-11-09: Aura targets keys at the FocusClient window. When EventClient denies processing within that subtree, WindowTargeter clears focus and returns no key target. [source] [Donor implementation](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window_targeter.cc#L175)
- Checked 2026-11-09: Manganese’s focus-request helper reinserts hidden registered-background windows. Its ordinary reach path requires workspace membership and does not dequeue a hidden scratchpad member. [source] [Focus reach scope](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1946) · [Focus reach scope](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1908) · [Focus reach scope](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1980)
- Checked 2026-11-09: The guide says hover does not raise. The WindowHovered reducer can reorder a newly focused float to the greatest ordinary depth within that workspace’s floating list; physical/native event delivery remains a separate boundary. [source] [Published policy and reducer order](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L71) · [Published policy and reducer order](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1627) · [Published policy and reducer order](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2062) · [Published policy and reducer order](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/workspace.ts#L279) · [Published policy and reducer order](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/placement.ts#L186)
- Checked 2026-11-09: During native keydown handling, a retained target without an app element clears the renderer’s local native keyboard target and requests page keyboard focus when the channel is bound. [source] [Missing-element target guard](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1281) · [Target validation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1295)
- 2026-11-09: Native app focus requests are cancelable; a separate release request reports the page element pressed outside native windows, letting shell controls retain keyboard focus without inferring popup dismissal (Domicile@checked-pin) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/app-element.ts#L18> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/app-element.ts#L39>
- 2026-11-04: scroll adds a focus_ring command (prev, next, first, last, set, remove) with focus_ring_length and focus_ring_wrap options, a recency ring distinct from sway focus commands (scroll@b6586d2) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L208-L215> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L542-L546>
- 2026-11-09: Aura already declares a typed attention-request property; a historical claim that urgency always requires a new property is broader than the checked source supports (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/client/aura_constants.h#L93>
- 2026-11-09: Browser-window guest focus events do not count as user reach while covered or behind a desktop panel; the same path excludes the shell’s own programmatic focus (Domicile@checked-pin) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/BrowserWindow.tsx#L377>
- 2026-11-09: Ash cycling normally preserves the active window by disabling widget activation; spoken feedback permits activation. The nonactivating path installs a root-scoped custom event targeter (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_cycle/window_cycle_list.cc#L378>
- 2026-11-05: Manganese focus movement on Meta with H, J, K, L or arrows wraps at the ends of a container [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L11>
- 2026-11-05: sway's no_focus criteria rule stops matching windows from taking focus on creation; it has no effect on the first window in a workspace (sway@1652c54) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L847-L849>
- 2026-11-05: Maintainer-merged precedents for window behavior fixes in 2026-10 and 2026-11 include #1071 (pointer kept on an app until press release), #1431 (repositioned popups), #1238 (old-client tiled maximize, citing sway) [docs] <https://github.com/cprussin/domicile/pull/1071> <https://github.com/cprussin/domicile/pull/1431> <https://github.com/cprussin/domicile/pull/1238>
- 2026-07-16: A July native-shell report distinguishes receiving Wayland keyboard events from activating the secondary Views root; the earlier BrowserWidget focus proof did not qualify that arrangement (Views@150.0.7871.124) [docs]
- 2026-11-04: At the 2026-11-04 snapshot focus follows the pointer by default; the docs say a shell wanting click-to-focus changes one case in reduceWindows (WINDOW-MANAGEMENT.md:62-64), which sits in a TypeScript package a shell would fork or wrap [docs]

## Where it touches Domicile

- `manganese`: Accepted focus requests reach the managed target’s workspace and tab. Browser-guest focus is filtered for covered windows, desktop panels and programmatic shell focus before it counts as user reach. [source] [Shell activation policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L64) · [Guest focus guards](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/BrowserWindow.tsx#L377)
- `compositor`: Activation tokens are checked against recent keyboard focus. Dropped requests can set urgency; an authorized request ignored by the shell remains silent. [source] [Activation-token policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L616)
- `sdk`: The window namespace exposes native focus, focus-change and focus-request events. Focus-release event detail separately identifies the page element pressed outside native windows. [source] [Native focus namespace](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/windows.ts#L18) · [Release-request detail](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/app-element.ts#L39)
- `engine:blink`: Clicking a native `<app>` emits a cancelable, bubbling focus request; the default focuses that client when the event remains unhandled. [source] [Native element focus default](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L502)
- Status: `landed` for the documented focus and activation behavior; the rail-hover slice is merged as [#1892](https://github.com/cprussin/domicile/pull/1892). The donor’s configurable urgency policy remains a separate row in the map. [source] [Pin behavior](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L22)

## Lineage

- 2026-11-06: entering a tab group’s empty rail was made a focus operation. [source] [#1892](https://github.com/cprussin/domicile/pull/1892)

## Sources

- S-9fe5a8a3f191 · 2026-09-21 · [Container commands and compositor configuration](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L1).
- S-204a969601d0 · 2026-11-06 · sway and Scroll semantic inventory.
- S-23c521755f22 · 2026-11-09 · [Grouped layouts, workspace homes and interaction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L1).
- S-fd0c87388c02 · 2026-11-08 · Public Domicile change catalog.
- S-e77ffff40807 · 2026-11-09 · [Shell entry, client elements and native-window control](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L1).
- S-543465ec2563 · 2026-11-06 · Mutter activation code at the donor pin.
- S-bf5c4cf3a78c · 2026-11-08 · Hyprland layout, rules and focus source at the donor pin.
- S-bc2a6582ae19 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-14b41dc319ce · 2026-11-09 · [Browser guest focus and reach policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/BrowserWindow.tsx#L1).
- S-8c438f028a0f · 2026-11-09 · [Desk-wide window reducer and workspace policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1).
- S-900dfbb65723 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-e1838c76399f · 2026-11-09 · [Workspace insertion, floating and focus policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/workspace.ts#L1).
- S-17c6354d2de5 · 2026-11-09 · [Window geometry, depth and fullscreen placement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/placement.ts#L1).
- S-1fa1acbff487 · 2026-11-09 · [Native window identity, bounds and focus namespace](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/windows.ts#L1).
- S-fbe8a9d8422d · 2026-11-09 · [Native focus, key forwarding and desktop events](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1).
