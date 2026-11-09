---
id: workspaces/workspace-model-and-homes
title: Workspaces and screen homes
kind: feature
family: workspaces
layers: [manganese, any-shell, compositor]
domicile_status: landed
domicile_refs: [cprussin/domicile#1946, packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, niri, ash, pinnacle, jay]
eras: []
rows: [SW-0126, SW-0157, SW-0158, SW-0159, SW-0160, SW-0161, SW-0162, SW-0164, SW-0165, SW-0166, SW-0167, SW-0171, SW-0228, SW-0243, SW-0244, SW-0245, SW-0246, SW-0248, SW-0249, SW-0273, SW-0281, SW-0283, SC-0057, SC-0058, SC-0059, SC-0061, SC-0110, SC-0111, SC-0112, SC-0113, SC-0114, SC-0115, SW-0486, SW-0487, SW-0488, SW-0489, SW-0490, SW-0491, SW-0492, SW-0493, SW-0494, SW-0495, SW-0496, SW-0497, SW-0498, SW-0499, SW-0500, SW-0501, SW-0502, SW-0503, SW-0504, SW-0505, SW-0506, SW-0507, SW-0510, SW-0511, SW-0512, SW-0513, SW-0514, SW-0515, SW-0516, SW-0517, SW-0653, SW-0654, SW-0655, SW-0656, SW-0657, SW-0667, SC-0367, SC-0368, SC-0369, SW-1016, SW-1021, SW-1022, SW-1023, SW-1024, SW-1030, SW-1032, SC-0544, SC-0545, SC-0546, SC-0547, SW-1062, SW-1063, SW-1064, SW-1065, SW-1066, SW-1067, SW-1148, SC-0660, SC-0662, SC-0663, SW-1223, SW-1224, SW-1225, SW-1226, SW-1227, SW-1228, SW-1229, SW-1230, SW-1231, SW-1232, SW-1233, SW-1234, SW-1235, SC-0689, SC-0690, SC-0691, SC-0692, SW-1295, SW-1296, SW-1297]
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: false
bucket: domicile
tags: [family:workspaces, layer:manganese, status:landed, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-215e62e641e8, S-23c521755f22, S-29c5c911bfd6, S-2a64ebf508b2, S-34997c703058, S-356e0cb0a114, S-37f72d290d44, S-48603351a6ee, S-4a2b900f6a8d, S-5ed695b4cddf, S-6ca6958d79ba, S-8c438f028a0f, S-902271613c39, S-9fe5a8a3f191, S-a486a03ed3e3, S-b28649c0301e, S-c3cf234a5550, S-e33fe2e31922, S-e419935214ca, S-e4ff18df1f16, S-f278a9071404, S-f88b70fd0dfa, S-fd0c87388c02]
donor_availability: chromeos-gated
---
# Workspaces and screen homes

Workspaces group windows, while a screen home records where a workspace is shown or returns when selected.

## Behavior

- Selecting a workspace already shown on another screen focuses that screen.
- Nonempty hidden workspaces retain a screen home; empty hidden workspaces open on the screen with the keyboard.
- Workspace swaps preserve identity while moving contents and repairing screen associations.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| sway @ pinned source | Named and numbered workspaces attached to outputs | Output rules can list fallback outputs; workspace selection has optional back-and-forth behavior. | [Workspace rules](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L931) |
| scroll @ pinned source | Workspaces containing scrolling rows | Overview exposes multiple workspaces and permits dragging windows between them. | [Overview and workspaces](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L249) |
| niri @ pinned source | Independent dynamic workspace lists per monitor | A bottom empty workspace is retained; named and unnamed workspaces differ in how they retain a disconnected monitor home. | [Workspace lifetime](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Workspaces.md#L3) |
| ash @ engine Chromium pin | One active desk spanning displays | Desk switching and window moves operate across display roots; per-output independent selection has different state. | [Desk definition](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.h#L29) |
| Pinnacle @ v0.2.4 | Many-to-many window tags | A window can carry multiple tags and an output can show several active tags. [docs] | [Release reference](https://github.com/pinnacle-comp/pinnacle/blob/v0.2.4/api/rust/src/tag.rs#L17) · [Release reference](https://github.com/pinnacle-comp/pinnacle/blob/v0.2.4/api/rust/src/tag.rs#L219) |
| Jay @ v1.15.0 | Separate workspace show, move and focus actions | An output argument can choose only the creation location; moving an existing workspace is a separate choice. [docs] | [Release reference](https://github.com/mahkoh/jay/blob/v1.15.0/book/src/workspaces.md#L25) |

## What the notes found

- Checked 2026-11-09: A workspace, a layout group and a physical display are separate units; donor terms are crosswalked in the [family glossary](../../04-sway-family/glossary-rows.tsv). [docs]
- Checked 2026-11-09: Ash desks span displays under one active desk, while Manganese associates each shown workspace with one screen. The names therefore do not denote identical state. [source] [Ash desk](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.h#L29) · [Manganese workspace](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L45)
- Checked 2026-11-09: The Ash donor controller belongs to the ChromeOS build; its behavior and the Linux shell’s implementation surface remain separate. [source] [Ash build gate](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/BUILD.gn#L16)
- Checked 2026-11-09: SDK virtual displays add connectorless monitors. Scroll’s similarly named virtual outputs instead divide an existing output into recursive workspace regions. [source] [Virtual display API](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/displays.ts#L21) · [Scroll regions](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1787)
- Checked 2026-11-09: Ash uses the same active desk container identifier across display roots; activation updates its per-root containers together. [source] [Donor implementation](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desks_util.cc#L112) · [Donor implementation](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.cc#L291) · [Donor implementation](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.cc#L486)
- Checked 2026-11-09: Ash restores all-desks windows’ relative stacking among ordinary windows per display root. Floated windows have separate association and stacking paths. [source] [Donor implementation](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.cc#L830) · [Donor implementation](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.cc#L852) · [Donor implementation](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.cc#L905) · [Donor implementation](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desks_util.cc#L136)
- Checked 2026-11-09: Selecting a hidden workspace uses its remembered screen, falling back to the focused screen when no home remains. An empty hidden workspace has no home. [source] [Home invariant](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L138) · [Selection](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2017)
- Checked 2026-11-09: Sending to another workspace moves the selected window or group tiled and leaves focus behind. Named-window relocation also arrives tiled without changing any screen’s shown workspace. Workspace swaps additionally rewrite the previous-workspace name to follow the exchanged contents. [source] [Selected-node send](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2426) · [Named-window relocation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2447) · [Swap correspondence](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1236)
- 2026-11-09: Ash uses one active desk container identifier on all display roots. Activation and deactivation update every associated container together, while per-root maps preserve container and stacking bookkeeping; the donor remains ChromeOS-gated (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desks_util.cc#L112> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.cc#L291> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.cc#L486>
- 2026-11-09: Ash activation animates across root windows and activates the target desk’s most recently used window. Desk observers cover add, remove, reorder, activation and naming (ash@150.0.7871.124) [source] <https://github.com/chromium/chromium/blob/150.0.7871.124/ash/wm/desks/desks_controller.h#L76> <https://github.com/chromium/chromium/blob/150.0.7871.124/ash/wm/desks/desks_controller.h#L221>
- 2026-11-09: Moving a minimized Ash window from the active desk restores it unless it is visible on all desks; the move API also names the target display root (ash@150.0.7871.124) [source] <https://github.com/chromium/chromium/blob/150.0.7871.124/ash/wm/desks/desks_controller.h#L243>
- 2026-10-02: A 2026-10-02 adapter study distinguishes global compositor workspace numbering from per-monitor names supplied by the shell; its generic protocol path has fewer commands than compositor-specific IPC (views@2026-10-02) [docs]
- 2026-07-18: Workspace ordering and naming are shell policy in the June audit; the July Ash study distinguishes global desks from independent per-output workspace rows (Chromium historical studies) [docs]
- 2026-10-02: The workspace strip distinguishes immediate local selection from request mode, where a click leaves selection unchanged until the compositor model’s observed snapshot updates it (views-shell@October-2026) [docs]
- 2026-05-26: The COWL workspace reference associates workspace groups with outputs and documents an initial group and workspace snapshot when subscriptions start (COWL historical client reference) [docs]
- 2026-10-02: A 2026-10-02 Views-shell account updated the workspace strip from compositor snapshots; requesting focus did not optimistically change it. A reported focus command became visible on the snapshot echo (Views shell@154.0.8037.92) [docs]

## Where it touches Domicile

- `manganese`: Ten fixed workspace names each appear on at most one screen. A populated hidden workspace retains its home while that screen remains; otherwise it is rehomed to the focused screen. Overview swaps exchange contents and homes while retaining workspace names. [source] [Workspace model](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L45) · [Home repair](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2546) · [Workspace swap](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L365) · [#1946](https://github.com/cprussin/domicile/pull/1946)
- `compositor`: displays are published independently of the shell’s workspace model. [source] [Desk contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md#L34)
- Status: `landed`; overview workspace swaps are merged and present in the pin documentation. [source] [#1946](https://github.com/cprussin/domicile/pull/1946)

## Lineage

- 2026-11-08: overview steering added directional workspace swaps with home-state repair. [source] [#1946](https://github.com/cprussin/domicile/pull/1946)

## Sources

- S-9fe5a8a3f191 · 2026-09-21 · [Container commands and compositor configuration](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L1).
- S-37f72d290d44 · 2026-11-05 · [Scrolling viewport and navigation semantics](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L1).
- S-23c521755f22 · 2026-11-09 · [Grouped layouts, workspace homes and interaction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L1).
- S-e4ff18df1f16 · 2026-11-09 · [Desk-wide page and display coordinate ownership](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md#L1).
- S-fd0c87388c02 · 2026-11-08 · Public Domicile change catalog.
- S-48603351a6ee · 2026-11-08 · Niri workspace contract at the donor revision.
- S-902271613c39 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-b28649c0301e · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-a486a03ed3e3 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-5ed695b4cddf · 2026-06-27 · Pinnacle release API documentation.
- S-e419935214ca · 2026-09-28 · Jay release documentation.
- S-f278a9071404 · 2026-11-09 · [Virtual display and monitor namespace](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/displays.ts#L1).
- S-4a2b900f6a8d · 2026-11-05 · [Scrolling layout, marks and scripting contract](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1).
- S-f88b70fd0dfa · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-c3cf234a5550 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-8c438f028a0f · 2026-11-09 · [Desk-wide window reducer and workspace policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1).
- S-2a64ebf508b2 · 2026-11-09 · [Ash target build gate at Domicile’s engine pin](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/BUILD.gn#L16).
