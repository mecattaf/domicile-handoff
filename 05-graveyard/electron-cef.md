---
id: 05-graveyard/electron-cef
title: "Electron and CEF as embedding donors"
kind: graveyard
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-d5eff5ea6a6e, S-6f807e7785c4, S-72038958dca1, S-c6f41daa6909]
---

# Electron and CEF as embedding donors

The March study compared Electron, CEF offscreen rendering and a direct Chromium embedder for desktop shell surfaces. [docs]

> Era and its assumptions: the March 2026 embedder comparison, which assumed the host would provide desktop surface roles and typed page access. For Domicile this probably adds little: Domicile already owns its engine fork and surface integration. Recorded because embedding patterns could inform a host without adopting its whole runtime.

## Dates

2026-03-26–27; evidence corrected in a 2026-09-27 synthesis. [docs]

## What it proved

The built COWL route was a native Content API embedder; the CEF OSR route remained a surveyed alternative. [docs]

## Key learnings

- Surface-role authority was distinct from frontend developer convenience. [docs]
- Estimated OSR latency was not a measured result. [docs]
- The donor matrix separates process startup, per-content callbacks, message transport and patch application without establishing the required shell surface role. [docs]

## Ending

The historical comparison selected direct Chromium ownership for the required surface roles. This is a dated scope decision, not a current verdict on Electron or CEF. [docs]

## What carried forward

[REVOLUTION](../01-chromium-lineage/revolution.md) and [fork maintenance](../01-chromium-lineage/mechanisms/tier-two-fork-rebase-cost.md) retain the mechanism findings.

## Sources

- S-6f807e7785c4 · 2026-09-27 · Embedder evidence corrections and comparison
- S-72038958dca1 · 2026-04-04 · Embedder lineage and mechanism comparison
- S-c6f41daa6909 · 2026-07-03 · Content-dated spring shell chronology
- S-d5eff5ea6a6e · 2026-03-31 month upper bound · Electron and CEF embedder-pattern comparison matrix
