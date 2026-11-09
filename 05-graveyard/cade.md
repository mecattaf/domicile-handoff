---
id: 05-graveyard/cade
title: "CADE"
kind: graveyard
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-90efe842b815, S-7f4e4b3b464a, S-8b05eb1370ff, S-a5d1670abc79, S-a16cc8bdfb57, S-0f934972e3ae, S-9eeaa5387ca1, S-fd16ec144654]
---

# CADE

CADE investigated Ash/Aura as the compositor, with DRM presentation and Exo application hosting on generic Linux. [docs]

> Era and its assumptions: CADE, which assumed Ash, Aura and Exo would own the scene and application hosting on Linux. For Domicile this probably adds little: Domicile already supplies its own engine and Smithay compositor boundary. Recorded because build availability, graphics initialization and physical presentation required separate evidence.

## Dates

April–June 2026; the June 10 VM probe, June 28 headless bring-up and June 29 physical-display results preserve distinct qualification boundaries. [docs]

## What it proved

The June 10 final report retracts scanout despite internal rendering. June 28 records stable graphics initialization with real scanout pending. June 29 then records physical-display modesetting, Exo clients and interactive typing/scrolling; a GTK buffer-import failure remained. These are historical observations, not a current Linux compatibility qualification. [docs]

## Key learnings

- ChromeOS build availability did not follow from a source file existing. [docs]
- Build, graphics initialization, scanout, protocol hosting and usable input were distinct evidence gates. [docs]
- The graphics backend below the service adapters required generic Linux integration. [docs]
- The design argument reports a later interactive session and identifies input/service reseating and crash scope as costs. [docs]

## Ending

The June 29 design argument selected the client-under-niri boundary after describing integration and robustness costs in the compositor-owning branch. [docs]

## What carried forward

The [Aura/Ash/Exo map](../02-aura-ash-exo/README.md) retains donor mechanisms; [Ozone findings](../01-chromium-lineage/mechanisms/ozone-platform-work.md) retain the graphics evidence limits.

## Sources

- S-a16cc8bdfb57 · 2026-06-28 · Generic Linux Ash and graphics bring-up results
- S-0f934972e3ae · 2026-06-29 · Ash build and display qualification boundary
- S-9eeaa5387ca1 · 2026-06-29 · CUBS compositor and toolkit axes
- S-fd16ec144654 · 2026-10-28 · Chromium and Aura lineage reconstruction
- S-a5d1670abc79 · 2026-06-10 · VM DRM and Exo shared-memory client result
- S-7f4e4b3b464a · 2026-06-29 · Physical-display modeset progression and runtime gates
- S-8b05eb1370ff · 2026-06-29 · Interactive Ash, Exo clients and remaining integration limits
- S-90efe842b815 · 2026-06-10 · VM compositor probe and final scanout retraction
