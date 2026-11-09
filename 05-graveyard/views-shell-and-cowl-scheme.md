---
id: 05-graveyard/views-shell-and-cowl-scheme
title: "views-shell and the cowl:// study"
kind: graveyard
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-80f21569978a, S-f86e13423d6e, S-fd16ec144654, S-c01d4f89b475]
---

# views-shell and the cowl:// study

The October study separated a browser-free Views client from a web system page and its privileged browser seam. [docs]

> Era and its assumptions: the October 2026 Views and system-page study, which assumed a native client or privileged page would supply shell surfaces. For Domicile this probably adds little: Domicile already provides page hosting and compositor interfaces. Recorded because page origin, native surface role and host authority were separate boundaries.

## Dates

October 2026; the standalone Views result is dated 2026-10-02. [docs]

## What it proved

A 2026-10-02 report records a browser-free standalone Views build, layer-shell surfaces, an acknowledged workspace action and injected keyboard input on a headless bench; no physical session ran it. The earlier Ash-lift prototype remained a separate uncompiled artifact. [docs]

## Key learnings

- A stock Views control could replace Ash wrapper code without inheriting Ash session coupling. [docs]
- A custom URL scheme did not by itself grant compositor authority. [docs]
- Surface role, native host authorization and page origin were different boundaries. [docs]

## Ending

Work stopped for reasons outside this library's scope; the technical state at that point was a headless standalone Views result plus a separate system-page study.

## What carried forward

[Layer-shell ownership](../01-chromium-lineage/mechanisms/layer-shell-client-and-compositor.md) and [the typed seam](../01-chromium-lineage/mechanisms/typed-page-host-seam.md) retain the mechanism distinction.

## Sources

- S-fd16ec144654 · 2026-10-28 · Chromium and Aura lineage reconstruction
- S-c01d4f89b475 · 2026-11-04 · Earlier compositor ideas compared with Domicile
- S-80f21569978a · 2026-10-02 · Standalone Views build and headless interaction result
- S-f86e13423d6e · 2026-10-01 · Recovered shell attempts and evidence boundaries
