---
id: 01-chromium-lineage/cubs
title: "CUBS: one browser process, a native Views shell"
kind: era
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-122550b9a732, S-9eeaa5387ca1, S-5d886dcdc3b2, S-8f764afc6722, S-fc035ad1863f, S-e77ffff40807, S-babbc1cf5372]
---

# CUBS: one browser process, a native Views shell

The June 29 CUBS design argument described a tier-2 Chromium fork whose browser process also hosted native Views shell surfaces under niri. It separated toolkit choice from compositor ownership. [docs]

## Dates and demonstrated scope

- The design argument chose Views for shell controls and niri for the compositor; CADE remained a distinct maximal branch. [docs]
- An earlier June 28 conceptual record and a separate June 29 shell-shape plan describe Ash-owned compositing. These planned scene-ownership boundaries differ from the client arrangement qualified by the June 30 Views result. [docs]
- A 2026-06-30 result reports a Views launcher painting on COWL's layer-shell-backed Aura root, receiving keyboard input and dismissing on Escape. The result rows were interactive demonstration targets, not application launch actions. [docs]
- A companion component-build comparison reports no renderer process for the Views surface and a renderer for the web surface. The exact memory and idle figures came from one sample per contender with unequal settle time. They do not measure Domicile. [docs]

## What carried forward

The experiment separated in-process Views painting from Mojo service/data transport. It also established that a layer-shell role can host a Views Widget without making Chromium the compositor. [docs]

> Era and its assumptions: CUBS, which assumed native shell surfaces justified a browser-process daemon under niri. For Domicile this probably adds little: Domicile already ships a web shell above its own Chromium-based scene. Recorded because toolkit cost and scene ownership are independent axes.


## Where it touches Domicile

[Unified binary](mechanisms/unified-binary.md) compares the historical process shape with [the Domicile binary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/THE-DOMICILE-BINARY.md); [shell authoring](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md) remains the current entry point. [docs]


## Sources

- S-9eeaa5387ca1 · 2026-06-29 · CUBS compositor and toolkit axes
- S-5d886dcdc3b2 · 2026-06-30 · Native Views layer-shell interaction result
- S-8f764afc6722 · 2026-06-30 · Single-sample native and web shell comparison
- S-122550b9a732 · 2026-06-29 · Ash-owned shell-shape plan and donor assumptions
- S-fc035ad1863f · 2026-11-09 pin check · Domicile launcher, process supervision and recovery boundaries
- S-e77ffff40807 · 2026-11-09 pin check · Pinned shell authoring contract
- S-babbc1cf5372 · 2026-06-28 · Conceptual Ash and Exo scene ownership with separate scrolling-layout policy
