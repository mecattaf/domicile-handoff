---
id: 01-chromium-lineage/revolution
title: "REVOLUTION: a purpose-built Chromium embedder"
kind: era
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-c6f41daa6909, S-72038958dca1, S-6f807e7785c4, S-514bff6c69a0]
---

# REVOLUTION: a purpose-built Chromium embedder

REVOLUTION reframed the shell host as a Chromium Content API embedder with direct Ozone work for Wayland surface roles. Electron and CEF supplied implementation references. [docs]

## Dates and demonstrated scope

- The content-dated chronology places the pivot on 2026-03-27 and COWL's name on 2026-03-28. [docs]
- The study separated native host integration from renderer-side HTML, CSS and TypeScript. [docs]
- It compared Ozone surface ownership, Content API delegates, V8 bindings and patch tooling. The CEF latency tables were estimates; they were not runtime results. [docs]

## What carried forward

[The typed seam](mechanisms/typed-page-host-seam.md), [layer-shell ownership](mechanisms/layer-shell-client-and-compositor.md) and [fork discipline](mechanisms/tier-two-fork-rebase-cost.md) became separate concerns. [docs]

> Era and its assumptions: REVOLUTION, which assumed the shell required its own embedder to control surface roles. For Domicile this probably adds little: Domicile already carries a desktop engine with native application elements. Recorded because the useful distinction is ownership of the surface rather than choice of frontend framework.


## Where it touches Domicile

[Engine-fork architecture](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md) defines the current engine boundary. The old embedder is a historical donor study. [docs]


## Sources

- S-c6f41daa6909 · 2026-07-03 · Content-dated spring shell chronology
- S-72038958dca1 · 2026-04-04 · Embedder lineage and mechanism comparison
- S-6f807e7785c4 · 2026-09-27 · Embedder evidence corrections and comparison
- S-514bff6c69a0 · 2026-11-09 pin check · Domicile engine integration and patch discipline
