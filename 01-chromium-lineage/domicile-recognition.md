---
id: 01-chromium-lineage/domicile-recognition
title: "Domicile: the shipped form recognized in November"
kind: era
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-c01d4f89b475, S-e4ff18df1f16, S-e77ffff40807, S-1a653077d862]
---

# Domicile: the shipped form recognized in November

A study written on 2026-11-04 recognized Domicile and Manganese as a shipped implementation of the recurring Chromium-desktop idea. That architectural correspondence does not itself qualify physical-display behavior. [docs]

## What changed

Domicile places Wayland application surfaces inside a web page, with Chromium rendering and a Smithay compositor delivering client surfaces. The earlier client-versus-compositor and native-versus-web alternatives therefore meet a different boundary. [docs]

## What carries forward

- Donor behavior and edge cases remain useful independently of the older binary that studied them. [inferred]
- The Aura/Ash/Exo map separates machinery already built into Chromium from ChromeOS-gated implementation and protocol semantics. [inferred]
- Browser-host seams, build records and closed era chapters retain their evidence limits. [inferred]

> Era and its assumptions: the 2026-11-04 recognition, which assumed earlier studies had to own the engine integration. For Domicile this probably adds little: Domicile already ships Wayland windows as page elements. Recorded because the old mechanisms can be compared at their actual layers without reviving their enclosing architecture.


## Where it touches Domicile

Current boundaries are [the architecture](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ARCHITECTURE.md), [one page for the desk](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md) and [shell authoring](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md). The earlier investigation supplies historical context for those current responsibilities. [docs]


## Sources

- S-c01d4f89b475 · 2026-11-04 · Earlier compositor ideas compared with Domicile
- S-e4ff18df1f16 · 2026-11-09 pin check · Domicile desktop-page and presenter architecture
- S-e77ffff40807 · 2026-11-09 pin check · Pinned shell authoring contract
- S-1a653077d862 · 2026-11-09 pin check · Pinned Domicile architecture and process boundary
