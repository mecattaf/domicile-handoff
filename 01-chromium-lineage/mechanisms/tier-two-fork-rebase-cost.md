---
id: 01-chromium-lineage/mechanisms/tier-two-fork-rebase-cost
title: "Tier-2 forks and rebase cost"
kind: mechanism
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-c07791e750f7, S-dfd7b6e6962d, S-9eeaa5387ca1, S-1c58e1373c0b, S-6f807e7785c4, S-333eaf1c44be, S-31f8b0a23334, S-514bff6c69a0, S-1c5da8dc640a]
---

# Tier-2 forks and rebase cost

The CUBS design argument retained the browser and added shell roles. The maintenance investigation distinguished additive files and small insertion seams from edits that replaced upstream implementation. [docs]

## Era and assumptions

> Era and its assumptions: COWL and CUBS, which assumed the project would maintain a browser fork across Chromium rolls. For Domicile this probably adds little: Domicile already maintains its own ordered patch series and engine releases. Recorded because diff shape describes maintenance exposure more reliably than an unmeasured calendar estimate.


## What the notes found

- COWL's quoted rebase budgets were estimates and differed across documents. They are not measured roll costs. [docs]
- The July recovery report records earlier producer and Wayland platform C++ code compiling at Chromium 150 without API changes while a small set of build-graph adjustments was required. This is one recovery result, not a guarantee across future pins. [docs]
- The accompanying upgrade log records upstream-refactor changes in broader browser patches. Compatibility of the selected Wayland bodies did not establish compatibility across the whole fork. [docs]
- A standalone Views prototype later distinguished drafted, lifted and compiled material. An API citation did not establish target visibility, link availability or runtime behavior. [docs]
- The October API migration record lists setter and background-factory changes across Chromium versions. A clean patch application therefore does not establish that additive files still compile against changed interfaces. [docs]

## Where it touches Domicile

[Engine-fork architecture](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md) independently describes minimizing edited upstream files, keeping additive code in its own tree and treating generated-list conflicts explicitly. [The engine measurements](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK-MEASUREMENTS.md) report their own limits. [docs]

The transferable finding is the separation between code availability, build-graph availability and exercised runtime behavior. [inferred]


## Sources

- S-c07791e750f7 · 2026-07-16 · Selected Wayland-body compatibility and broader browser-patch rebase changes
- S-dfd7b6e6962d · 2026-06-16 · Browser and shell client revival
- S-9eeaa5387ca1 · 2026-06-29 · CUBS compositor and toolkit axes
- S-1c58e1373c0b · 2026-07-16 · Native shell recovery and live widget limits
- S-6f807e7785c4 · 2026-09-27 · Embedder evidence corrections and comparison
- S-333eaf1c44be · 2026-10-01 · Views prototype draft, lift and compilation distinctions
- S-31f8b0a23334 · 2026-10-31 month upper bound · Views API migration record
- S-514bff6c69a0 · 2026-11-09 pin check · Domicile engine integration and patch discipline
- S-1c5da8dc640a · 2026-11-09 pin check · Domicile published engine measurements and their limits
