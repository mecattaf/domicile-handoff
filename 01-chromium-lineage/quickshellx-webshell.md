---
id: 01-chromium-lineage/quickshellx-webshell
title: "quickshellX / webshell: web content under niri"
kind: era
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-06b83f1982b4, S-ab96089cfecd, S-c6f41daa6909, S-dfe38f275826, S-fd16ec144654, S-e4ff18df1f16]
---

# quickshellX / webshell: web content under niri

quickshellX added QtWebEngine and QtWebChannel to a Quickshell host. It rendered web shell surfaces while niri retained window placement and compositor effects. [docs]

## Dates and demonstrated scope

- A content-dated chronology records transparent web content under niri on 2026-03-14 and system-data bridges on 2026-03-25. These are historical reports, not results rerun on Domicile. [docs]
- The web view remained clipped to its host surface. A menu leaving a narrow panel required another host surface or a different surface boundary. [docs]
- Qt and Chromium lifetimes, serialization and allocator assumptions remained part of the host integration. [docs]

## What carried forward

The typed data/operation/subscription seam survived. COWL replaced the host with a Chromium embedder and gave the browser-side code direct control of shell surface creation. [docs]

> Era and its assumptions: quickshellX / webshell, which assumed each panel was a separate native surface. For Domicile this probably adds little: Domicile already lays out the desk in one page. Recorded because a page can escape a panel only within the surface or page that owns its pixels.


## Where it touches Domicile

[The web-shell ceiling](mechanisms/web-shell-ceiling.md) compares the old clipping boundary with [one page for the desk](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md). [docs]


## Sources

- S-c6f41daa6909 · 2026-07-03 · Content-dated spring shell chronology
- S-dfe38f275826 · 2026-03-21 · Consolidated December web-shell contract
- S-fd16ec144654 · 2026-10-28 · Chromium and Aura lineage reconstruction
- S-06b83f1982b4 · 2026-03-25 · Web shell and system-service bridge result
- S-ab96089cfecd · 2026-03-26 · Host-surface clipping and dropdown limits
- S-e4ff18df1f16 · 2026-11-09 pin check · Domicile desktop-page and presenter architecture
