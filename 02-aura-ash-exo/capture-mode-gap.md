---
id: "02-aura-ash-exo/capture-mode-gap"
title: "Capture-mode coverage and current capture delivery"
kind: "topic"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile"]
sources: [S-1f554a1713a5, S-73bd30934d13, S-eecd68f7af94, S-5cf90666708b, S-555ff6bd47c4, S-7ef9098429bf, S-c2596d00b100, S-514bff6c69a0]
---

# Capture-mode coverage and current capture delivery

The July lift survey did not fully inventory the Ash capture-mode subtree. That is a coverage limit in the historical map, rather than a current Domicile capture-status finding. [docs]

## Chromium donor boundary

The current [browser capture-mode target](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/ash/capture_mode/BUILD.gn#L5) and [recording service](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chromeos/ash/services/recording/BUILD.gn#L5) both require ChromeOS. Their presence in Chromium does not establish retention in Domicile's Linux engine build. [source]

The [capture delegate](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/public/cpp/capture_mode/capture_mode_delegate.h#L89) spans storage, policy, audio, camera, session state and service launch. A controller transplant therefore crosses service contracts beyond selecting pixels. [source]

Recording selects a [frame-sink and subtree target](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chromeos/ash/services/recording/video_capture_params.cc#L284). A window moving to another display can [change the target and constraints](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chromeos/ash/services/recording/video_capture_params.cc#L112). These are capture-source and lifecycle facts; they establish neither a portal route nor a copy cost. [source]

## Domicile at the pin

Domicile documents monitor, window and region capture through its compositor and portal path. Browser-window capture is a separate engine-frame-sink source; the Wayland toplevel list does not list browser windows. [docs] [Current capture contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L499), [browser capture](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md#L327).

The shell answers source-picking requests. The engine supplies rendered frames, the compositor controls delivery, and recording is handled separately. Capture behavior and permission boundaries are the comparison; the inspected target, delegate and recording sources do not qualify the whole Ash capture implementation. [inferred]

## Feature links

- [Screen and window capture](../06-features/capture/screencopy-and-image-capture.md)
- [Foreign toplevels](../06-features/capture/foreign-toplevel-and-taskbar.md)

## Sources

- S-1f554a1713a5 · 2026-07-18 · Lift-map evaluation and uncovered capture-mode scope
- S-73bd30934d13 · 2026-07-18 · Exo core and capture-adjacent inventory
- S-eecd68f7af94 · 2026-11-09 · Chromium browser capture-mode target and dependency gate
- S-5cf90666708b · 2026-11-09 · Chromium recording-service target gate
- S-555ff6bd47c4 · 2026-11-09 · Chromium capture delegate service contract
- S-7ef9098429bf · 2026-11-09 · Chromium recording frame-sink targets and display movement
- S-c2596d00b100 · 2026-11-09 pin check · Pinned desktop portal contract
- S-514bff6c69a0 · 2026-11-09 pin check · Domicile engine integration and patch discipline
