---
id: 01-chromium-lineage/july-native-shell-floor
title: "The July 2026 native-shell floor"
kind: era
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-c07791e750f7, S-1c58e1373c0b, S-fd16ec144654, S-24f0ff5009b7]
---

# The July 2026 native-shell floor

The July continuation rebuilt a native Views shell on Chromium 150 under niri. It reported rendered bar and launcher surfaces, with explicit qualification limits for secondary-widget focus, popup parenting and display capture. [docs]

## Dates and demonstrated scope

- A 2026-07-16 report records the earlier producer code and Wayland platform code compiling without C++ API changes; the listed adjustments were in the build graph. [docs]
- The report records bar and launcher rendering and identifies live-only errors in view lifecycle, accessibility naming and environment assumptions. [docs]
- The upgrade log qualifies its July 16 bar capture as a virtual-output result. A physical-GPU scanout and capture experiment remained unrun in that account. [docs]
- The June 30 BrowserWidget weld did not settle secondary shell-widget focus: Wayland keyboard events reached the surface but stopped before the focused Views root. [docs]
- The later October reconstruction distinguishes the July read-only workspace rail from the June web switcher that actually changed workspaces. [docs]

## Handoff

A surface that paints is not evidence that its controls receive input or that its workspace model acts on the compositor. The component and lifecycle notes preserve these boundaries. [inferred]

> Era and its assumptions: the July 2026 native-shell floor, which assumed a native Chromium shell would coexist with niri. For Domicile this probably adds little: Domicile already provides page layout, SDK state and a separate compositor delivery path. Recorded because build graph compatibility and runtime widget invariants are different kinds of evidence.


## Where it touches Domicile

The [build feature](../06-features/build-and-packaging/nix-and-engine-builds.md) carries generic packaging lessons; [shell configuration](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-CONFIG.md) documents the current shell boundary. [docs]


## Sources

- S-c07791e750f7 · 2026-07-16 · Native shell engine upgrade and virtual-output rendering boundary
- S-1c58e1373c0b · 2026-07-16 · Native shell recovery and live widget limits
- S-fd16ec144654 · 2026-10-28 · Chromium and Aura lineage reconstruction
- S-24f0ff5009b7 · 2026-11-09 pin check · Pinned shell configuration contract
