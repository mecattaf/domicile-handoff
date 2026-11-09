---
id: 01-chromium-lineage/mechanisms/ozone-platform-work
title: "Ozone platform work"
kind: mechanism
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-982a5344a43a, S-90efe842b815, S-7f4e4b3b464a, S-8b05eb1370ff, S-81c276c60385, S-a16cc8bdfb57, S-0f934972e3ae, S-1c58e1373c0b, S-1c5da8dc640a, S-703c0d408b29]
---

# Ozone platform work

The lineage used Ozone for two different jobs: creating Wayland client surfaces and presenting a Chromium-owned scene directly through DRM. A working backend for one job did not qualify the other. [docs]

## Eras and mechanisms

- COWL added a layer-shell platform-window role, carrying configure-before-buffer, anchoring, keyboard-interactivity and exclusive-zone state. [docs]
- The patch campaign exposed an opaque-region declaration that prevented correct compositing of translucent content. The retained patch cleared that declaration; alpha pixels alone did not describe the surface's opacity contract. The global patch is historical, without a claim of current applicability. [docs]
- CADE booted a ChromeOS-target Ash build with the DRM backend and fake service clients. Generic Linux GBM selection was a separate graphics integration issue. [docs]
- The June 10 VM report retracts scanout despite DRM master and internal rendering: the active display retained its previous framebuffer. Rendering and presentation were separate gates. [docs]
- The headless result records stable system Mesa GBM initialization and still distinguishes a real display sink from an initialized GPU process. [docs]
- June 29 physical-display bring-up separately records runtime display gates and initialization order, followed by modesetting and rendering. Stable GPU initialization did not by itself trigger physical presentation. [docs]

> Era and its assumptions: COWL and CADE, which assumed the fork would add surface roles or drive DRM directly. For Domicile this probably adds little: Domicile already carries Ozone integration and desktop presenters. Recorded because platform initialization, surface role, transparency and physical presentation require different evidence.


## Evidence limits

The retained runtime findings are historical reports. Pinned Domicile source and published measurements establish the corresponding current boundaries. [docs]

## Where it touches Domicile

[Window compositing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md) and [engine-fork measurements](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK-MEASUREMENTS.md) define Domicile's own surface and presentation evidence. Offscreen copies do not by themselves qualify lit-display scanout. [docs]


## Sources

- S-81c276c60385 · 2026-06-29 · Spring and June compositor lineage reconstruction
- S-a16cc8bdfb57 · 2026-06-28 · Generic Linux Ash and graphics bring-up results
- S-0f934972e3ae · 2026-06-29 · Ash build and display qualification boundary
- S-1c58e1373c0b · 2026-07-16 · Native shell recovery and live widget limits
- S-7f4e4b3b464a · 2026-06-29 · Physical-display modeset progression and runtime gates
- S-8b05eb1370ff · 2026-06-29 · Interactive Ash, Exo clients and remaining integration limits
- S-90efe842b815 · 2026-06-10 · VM compositor probe and final scanout retraction
- S-982a5344a43a · 2026-06-27 source bound · Retained client opaque-region patch
- S-1c5da8dc640a · 2026-11-09 pin check · Domicile published engine measurements and their limits
- S-703c0d408b29 · 2026-11-09 pin check · Domicile native surface composition and presentation boundaries
