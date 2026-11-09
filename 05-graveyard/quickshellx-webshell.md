---
id: 05-graveyard/quickshellx-webshell
title: "quickshellX / webshell"
kind: graveyard
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-06b83f1982b4, S-ab96089cfecd, S-c6f41daa6909, S-dfe38f275826, S-fd16ec144654]
---

# quickshellX / webshell

A QtWebEngine host rendered a web shell under niri. [docs]

> Era and its assumptions: quickshellX / webshell, which assumed a Qt host allocated panel surfaces under an external compositor. For Domicile this probably adds little: Domicile already places the desk in one page spanning displays. Recorded because a panel clipping limit belonged to native host geometry.

## Dates

2026-03-14–26 in the content-dated chronology. [docs]

## What it proved

The chronology records transparent web rendering and real system-data bridges, while retaining an external compositor. [docs]

## Key learnings

- The web view was clipped to its native surface. [docs]
- A menu outside a panel required host-managed surface allocation. [docs]
- The typed service contract was more durable than the host framework. [docs]

## Ending

The recorded weakness was surface geometry: content could not leave a narrow panel without native host support. [docs]

## What carried forward

COWL took direct ownership of surface creation; [the surface ceiling](../01-chromium-lineage/mechanisms/web-shell-ceiling.md) compares the boundary with Domicile.

## Sources

- S-c6f41daa6909 · 2026-07-03 · Content-dated spring shell chronology
- S-dfe38f275826 · 2026-03-21 · Consolidated December web-shell contract
- S-fd16ec144654 · 2026-10-28 · Chromium and Aura lineage reconstruction
- S-06b83f1982b4 · 2026-03-25 · Native Qt-host data bridges and remaining system-integration limits
- S-ab96089cfecd · 2026-03-26 · Host-surface clipping and runtime recovery account
