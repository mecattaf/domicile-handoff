---
id: 01-chromium-lineage/mechanisms/unified-binary
title: "The unified-binary insight"
kind: mechanism
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-f3843126f2e0, S-8e319e25bda8, S-dfd7b6e6962d, S-9eeaa5387ca1, S-5d886dcdc3b2, S-fc035ad1863f]
---

# The unified-binary insight

The earlier browser-and-shell designs aimed to share Chromium's browser services, GPU process and native toolkit. A shared product entry point, shared engine and shared operating-system process are separate claims. [inferred]

## Eras

COWL explored browser and shell surfaces in one fork. The CUBS design argument made the browser process a shell daemon with Views controls, retaining niri outside that process tree. CADE instead made Chromium the compositor. [docs]

> Era and its assumptions: COWL and CUBS, which assumed browser and shell integration required a unified fork with a browser-process daemon. For Domicile this probably adds little: Domicile already provides one launch entry point with supervised engine and compositor children. Recorded because product unification does not require process fusion.


An early COWL architecture account described per-surface renderers and store instances communicating through browser-routed messages. A shared browser and GPU did not imply one shared JavaScript store. [docs]

The CUBS design explicitly kept normal sandboxed renderers while placing native shell widgets in the browser process. A single product binary did not imply a single process. [docs]

## Evidence boundary

The June Views result records sharing the browser-process native toolkit without a renderer per shell surface. It does not measure Domicile or establish that all desktop services belong in that process. [docs]

## Where it touches Domicile

[The Domicile binary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/THE-DOMICILE-BINARY.md) describes a Rust launcher supervising distinct engine and compositor children. It also documents the asymmetric recovery of the two children. [docs]

These failure domains are the current boundary. The older unified-binary chapter contributes a vocabulary for shared services and lifecycle, not a replacement process model. [inferred]


## Sources

- S-f3843126f2e0 · 2026-05-31 month upper bound · Client architecture with per-surface renderer stores and browser-routed messages
- S-dfd7b6e6962d · 2026-06-16 · Browser and shell client revival
- S-9eeaa5387ca1 · 2026-06-29 · CUBS compositor and toolkit axes
- S-5d886dcdc3b2 · 2026-06-30 · Native Views layer-shell interaction result
- S-fc035ad1863f · 2026-11-09 · Domicile launcher, process supervision and recovery boundaries
- S-8e319e25bda8 · 2026-06-30 month upper bound · CUBS browser-process shell and multi-process design
