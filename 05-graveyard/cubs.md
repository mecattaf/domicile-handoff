---
id: 05-graveyard/cubs
title: "CUBS and the July native-shell floor"
kind: graveyard
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-9eeaa5387ca1, S-5d886dcdc3b2, S-8f764afc6722, S-1c58e1373c0b]
---

# CUBS and the July native-shell floor

The CUBS design argument combined browser services and native Views shell surfaces in a tier-2 Chromium fork under niri. [docs]

> Era and its assumptions: CUBS and the July 2026 native-shell floor, which assumed native Views controls would run in the browser process under niri. For Domicile this probably adds little: Domicile already renders its shell through the desktop page. Recorded because toolkit painting did not qualify secondary-widget focus and lifecycle.

## Design argument

June 29–30 2026; native-shell continuation in July 2026. [docs]

## The July 2026 native-shell floor

The June Views result records painting and keyboard interaction without a WebContents renderer for the shell surface; the July continuation records a Chromium 150 build and rendered secondary shell widgets. [docs]

## Key learnings

- Toolkit choice and compositor ownership were independent axes. [docs]
- A Views surface could avoid a renderer process in the recorded arrangement. [docs]
- A BrowserWidget focus result did not qualify secondary shell-widget activation. [docs]
- Build-graph recovery and runtime lifecycle errors were separate work. [docs]

## Ending

Work stopped for reasons outside this library's scope; the technical state at that point included rendered native shell surfaces and explicit focus, popup and capture limits.

## What carried forward

The [era chapter](../01-chromium-lineage/cubs.md) and [July continuation](../01-chromium-lineage/july-native-shell-floor.md) preserve the findings.

## Sources

- S-9eeaa5387ca1 · 2026-06-29 · CUBS compositor and toolkit axes
- S-5d886dcdc3b2 · 2026-06-30 · Native Views layer-shell interaction result
- S-8f764afc6722 · 2026-06-30 · Single-sample native and web shell comparison
- S-1c58e1373c0b · 2026-07-16 · Native shell recovery and live widget limits
