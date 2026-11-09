---
id: 05-graveyard/cowl
title: "COWL, its revival and patch campaign"
kind: graveyard
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-966ffabac4e2, S-41ce2fd9abfe, S-8b956f5955ba, S-dfd7b6e6962d, S-81c276c60385, S-6f807e7785c4, S-5d886dcdc3b2]
---

# COWL, its revival and patch campaign

COWL was a native Chromium embedder and browser-plus-shell client under niri. [docs]

> Era and its assumptions: COWL, which assumed patched Ozone created separate shell-client surfaces under niri. For Domicile this probably adds little: Domicile already owns the desktop scene containing native application layers. Recorded because control messages, pixel transport and compositor state had different owners.

## First COWL

March–April 2026 established the embedder and surface-role design. A retained Chromium 148 handoff separately reports compilation with an initial launch outstanding. A March 30 diagnostic account reports responsive JavaScript evaluation, alongside service calls crashing the renderer; it does not qualify visible shell rendering. [docs]

## Revival

The 2026-06-16 revival restored the browser-plus-shell client boundary under niri. [docs]

## Patch campaign

The 2026-06-26–27 campaign records live web shell surfaces and transparency bring-up. [docs]

## What it proved

The lineage records browser-engine shell surfaces using patched Ozone layer-shell while niri retained placement. [docs]

## Key learnings

- Mojo controlled services; Wayland carried pixels; compositor IPC supplied desktop state. [docs]
- A translucent buffer did not override an opaque-region declaration. [docs]
- Assertion policy and surface lifecycle affected runtime behavior after compilation. [docs]
- CEF was surveyed as a host and retained as a donor; COWL itself was not CEF OSR. [docs]

## Ending

The construction model changed toward native Views after the client path had rendered. The record also identifies a permanent fork-maintenance surface; estimated rebase budgets are not measured costs. [docs]

## What carried forward

The [Views weld](../01-chromium-lineage/mechanisms/views-on-layer-shell.md) reused the Ozone host; [fork discipline](../01-chromium-lineage/mechanisms/tier-two-fork-rebase-cost.md) retained the generic maintenance lesson.

## Sources

- S-dfd7b6e6962d · 2026-06-16 · Browser and shell client revival
- S-81c276c60385 · 2026-06-29 · Spring and June compositor lineage reconstruction
- S-6f807e7785c4 · 2026-09-27 · Embedder evidence corrections and comparison
- S-5d886dcdc3b2 · 2026-06-30 · Native Views layer-shell interaction result
- S-8b956f5955ba · 2026-06-27 · Chromium client surfaces and keyboard qualification
- S-41ce2fd9abfe · 2026-05-31 month upper bound · Chromium 148 compilation and outstanding first launch
- S-966ffabac4e2 · 2026-03-30 · Process diagnostics and unresolved native service calls
