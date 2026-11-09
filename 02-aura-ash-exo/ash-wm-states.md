---
id: "02-aura-ash-exo/ash-wm-states"
title: "Ash window states"
kind: "collection"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile", "evidence:source"]
sources: [S-1f554a1713a5, S-2a64ebf508b2, S-327f70253fe5, S-6da8864cbb54, S-811958ec85ad, S-b53819d79c5b, S-d44c4b3b366b, S-a59107f9d164, S-e2a96179b3b7]
---

# Ash window states

Ash records state transitions, restore geometry and client-controlled bounds changes; these are ChromeOS-gated controller mechanisms. [docs]

## What Chromium already has

The [row data](rows/aura-lift.tsv) holds 24 rows for this beat: ADAPT 13, FRESH 4, LIFT 7. These are historical source verdicts. [docs]

Donor availability in those rows: `chromeos-gated` 24. Zero Ash references in a file do not bypass its target build gate. [docs]

- Window-positioning helpers still include Ash picture-in-picture and root-controller calls; pure geometry functions do not make the entire file portable. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_positioning_utils.cc#L83), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_positioning_utils.cc#L258).
- The current WindowResizer body contains geometry and aspect-ratio calculation without direct Shell access; its build availability still follows the Ash target gate. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_resizer.cc#L159), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_resizer.cc#L514).
- Ash bounds animation skips animation when the window layer already has a nonidentity target transform. Its crossfade branch recreates the layer tree, resizes the window, and animates with the retained old layers. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L1147), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L1229).
- WindowState delegates transitions to State::OnWMEvent; replacing its State object is an explicit policy seam. Client-controlled changes are asynchronous requests unless direct-bounds methods bypass the client. [source] [Donor source](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/window_state.h#L80), [Donor source](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/window_state.h#L401).

## Domicile at the pin

- `sdk and compositor`: The SDK separates client requests from states the shell sets, including tiled edges, maximized and fullscreen flags. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/window-state.ts#L5).
- `manganese`: The native window element sends current states through the SDK and suppresses state changes for a window already gone. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/AppWindow.tsx#L226).

## Boundary

Ash mutates an Aura window tree. Domicile lets the shell decide requested states and sends native client state through its compositor; identical names do not imply identical transition policy. [inferred]

## Feature links

- [aura-wm/window-states](../06-features/aura-wm/window-states.md)

## Sources

- S-d44c4b3b366b · 2026-07-18 · Chromium ash window states source and equivalence inventory
- S-1f554a1713a5 · 2026-07-18 · Lift-map evaluation and corrections
- S-2a64ebf508b2 · 2026-11-09 · Pinned donor interface inspection
- S-327f70253fe5 · 2026-11-09 · Pinned donor interface inspection
- S-6da8864cbb54 · 2026-11-09 · Pinned donor interface inspection
- S-811958ec85ad · 2026-11-09 · Pinned donor interface inspection
- S-b53819d79c5b · 2026-11-09 · Pinned donor interface inspection
- S-a59107f9d164 · 2026-11-09 pin check · Pinned native-window state and request enumeration
- S-e2a96179b3b7 · 2026-11-09 pin check · Pinned native-window element behavior
