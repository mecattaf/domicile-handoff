---
id: workspaces/overview
title: Workspace and window overview
kind: feature
family: workspaces
layers: [manganese, engine:blink]
domicile_status: landed
domicile_refs: [cprussin/domicile#1931, cprussin/domicile#1940, cprussin/domicile#1941, cprussin/domicile#1946, packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: true
scroll_parity: baseline
donors: [scroll, niri, ash]
eras: []
rows: [SC-0079, SC-0080, SC-0294, SC-0295, SC-0296, SC-0297, SC-0298, SC-0462, SC-0515, SC-0516, SC-0517, SC-0518, SC-0644, SC-0645, SC-0646, SC-0686, SC-0687, SC-0688, SC-0707]
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: false
bucket: domicile
tags: [family:workspaces, layer:manganese, status:landed, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-0d9c2ee30def, S-23c521755f22, S-2a64ebf508b2, S-37f72d290d44, S-3f3c8ab94769, S-4a6520490f9a, S-4ea478cdaf86, S-853bfd8f3db6, S-8c438f028a0f, S-8d9c52f81df5, S-a3bd9859358a, S-a486a03ed3e3, S-c3368d7ab705, S-da7f8d1b3bce, S-fd0c87388c02]
donor_availability: chromeos-gated
---
# Workspace and window overview

Overview shows live windows in workspace tiles and supports selecting, moving and steering among those tiles.

## Behavior

- Opening overview changes the camera from the visible workspace into a workspace grid.
- Picking a tile returns to that workspace; dragging moves a window between workspaces.
- Directional aim and workspace swaps operate within overview; unrelated window-editing commands can be suppressed.
- Reversing a transition midway preserves its current position.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| scroll @ pinned source | Current-workspace and all-workspace overview | The overview keeps normal window interaction and cross-workspace dragging. | [Overview](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L249) |
| niri @ pinned source | Zoomed-out workspace view with live operations | All keyboard bindings remain active; dragging and scrolling can navigate or create workspace targets. | [Overview interaction](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Overview.md#L14) |
| ash @ notes Chromium pin | Scoped transforms and transient descendants | Targeting is blocked during overview and restored on exit; minimized windows use mirrored layers. | [Transform lifecycle](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/overview/scoped_overview_transform_window.h#L37) · [Event targeting](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/overview/scoped_overview_transform_window.h#L206) |

## What the notes found

- Checked 2026-11-09: Manganese overview and Scroll overview share live-window navigation but differ in command availability: Manganese suppresses other window and workspace edits while overview is open. [source] [Command suppression](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L369) · [Scroll interaction](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L249)
- Checked 2026-11-09: Ash’s overview helper couples live-window transforms to transient descendants, event-target suppression and restoration. The Linux shell’s DOM transform contract is a separate implementation surface. [source] [Ash lifecycle](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/overview/scoped_overview_transform_window.h#L121) · [Domicile overview](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L333)
- Checked 2026-11-09: The Ash donor controller belongs to the ChromeOS build; its behavior and the Linux shell’s implementation surface remain separate. [source] [Ash build gate](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/BUILD.gn#L16)
- Checked 2026-11-09: Ash previews exclude popup-type transient windows, defer unparented transient children and remove all mirrors when the main source window is destroyed. Mirroring also keeps a source force-visible for occlusion tracking; minimized, tucked and inactive-desk layers are made visible at full opacity in the mirror. These are ChromeOS-gated donor lifetimes. [source] [Preview lifetime](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_preview_view.cc#L117) · [Source destruction](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_preview_view.cc#L165) · [Mirror visibility](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_mirror_view.cc#L192)
- Checked 2026-11-09: Overview aim survives only while overview is open and its target workspace has a home. Clearing that condition forgets the aim so steering returns to the shown workspace. [source] [Aim lifetime](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1222)
- Checked 2026-11-09: Toggling overview closes the launcher and clipboard panel and clears active drag state. Explicit show uses that transition only when overview was closed. [source] [Show and toggle](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1486)
- 2026-11-09: At entry, the overview helper records target opacity and target clip. Animated restoration can skip resetting the transform; nonanimated and minimized paths reset it directly. Empty original clips are removed after animation (Ash@engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L144> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L242> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L295>
- 2026-11-09: The overview helper holds per-transient event-targeting blockers and temporarily disables bubble offscreen adjustment. Added transient children receive their own scope; removing them releases it (Ash@engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L118> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L589> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L612>
- 2026-11-09: Ash overview restores window state and tracks transient descendants; minimized windows use mirrored layers. This is a semantic comparison point for live DOM transforms (ash@150.0.7871.124) [source] <https://github.com/chromium/chromium/blob/150.0.7871.124/ash/wm/overview/scoped_overview_transform_window.h#L37> <https://github.com/chromium/chromium/blob/150.0.7871.124/ash/wm/overview/scoped_overview_transform_window.h#L121>
- 2026-11-09: Ash overview changes live window transforms and also references Ash shadow and split-view controllers; this is a mechanism reference rather than a directly available Linux overview controller (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L259> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L487>
- 2026-11-09: The overview helper blocks ordinary event targeting for the managed window and transient descendants, then restores targeting when overview ends (ash@150.0.7871.124) [source] <https://github.com/chromium/chromium/blob/150.0.7871.124/ash/wm/overview/scoped_overview_transform_window.h#L206>
- 2026-11-09: Overview restores a separate drag-time tree synchronizer before the main tree synchronizer, covering drag starts without matching ends (Ash@engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L707>
- 2026-07-18: The July overview study distinguishes a current-desk window grid from a canvas showing every workspace simultaneously; live window transforms are a narrower shared mechanism (Chromium historical studies) [docs]
- 2026-06-27: The June Views inventory distinguishes an overview grid frame from access to live compositor-owned client windows; a browser-hosted Views client requires the compositor to supply external-window imagery and placement (Chromium historical studies) [docs]

## Where it touches Domicile

- `manganese`: Overview places live windows in per-screen workspace tiles, applies the camera transforms and clips, and owns aim, swaps and one-way show/hide commands. [source] [Overview policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L333) · [One-way commands](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L44) · [#1940](https://github.com/cprussin/domicile/pull/1940) · [#1941](https://github.com/cprussin/domicile/pull/1941)
- `engine:blink`: The built-in window elements embed live client and browser windows in the page; native `<app>` elements participate in CSS transforms. [source] [Built-in window elements](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L56) · [CSS window transforms](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L60)
- Status: `landed`; [#1931](https://github.com/cprussin/domicile/pull/1931), [#1940](https://github.com/cprussin/domicile/pull/1940) and [#1946](https://github.com/cprussin/domicile/pull/1946) are merged and reflected in the pin docs. [source]

## Lineage

- 2026-11-08: the overview landed, followed by reversible camera movement and directional steering. [source] [#1931](https://github.com/cprussin/domicile/pull/1931) · [#1940](https://github.com/cprussin/domicile/pull/1940) · [#1946](https://github.com/cprussin/domicile/pull/1946)

## Sources

- S-37f72d290d44 · 2026-11-05 · [Scrolling viewport and navigation semantics](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L1).
- S-23c521755f22 · 2026-11-09 · [Grouped layouts, workspace homes and interaction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L1).
- S-fd0c87388c02 · 2026-11-08 · Public Domicile change catalog.
- S-a3bd9859358a · 2026-11-08 · Niri overview contract at the donor revision.
- S-4a6520490f9a · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-a486a03ed3e3 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-0d9c2ee30def · 2026-11-09 · [Typed shell command parsing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L1).
- S-8d9c52f81df5 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-c3368d7ab705 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-8c438f028a0f · 2026-11-09 · [Desk-wide window reducer and workspace policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1).
- S-2a64ebf508b2 · 2026-11-09 · [Ash target build gate at Domicile’s engine pin](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/BUILD.gn#L16).
