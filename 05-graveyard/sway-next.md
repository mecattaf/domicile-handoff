---
id: graveyard/sway-next
title: The sway-next compositor study
kind: graveyard
layers: ["compositor", "any-shell"]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
bucket: domicile
evidence_mix: mostly-source
sources: ["S-abb5938fade4", "S-5e377e6e9b90", "S-aecf1c64f5c9"]
tags: ["bucket:domicile", "evidence:docs", "donor:sway", "donor:scroll", "era:sway-next"]
---

# The sway-next compositor study

The 2026-10-22..23 sway-next study explored a compositor fork that kept sway-compatible commands while retaining scroll layout, overview, jump and Lua semantics. [docs]

> Era and its assumptions: sway-next, which assumed layout, effects and configuration compatibility lived in one compositor fork. For Domicile this probably adds little: Domicile already exposes client windows to its web shell. Recorded because command compatibility and rendered behavior proved to be separate contracts.

## What it established

- The recorded fork combined sway socket and command-name compatibility with the inherited scrolling-layout implementation. [docs]
- Tabbed columns had a data model, arrangement and commands. Software-renderer screenshots recorded tabbed and stacked strip cells; pointer interaction, dragging and cycling remained incomplete or unqualified, and the capability flag stayed off. [docs]
- Saved-space restoration did not retain the column mode. [docs]
- The GLES2 blur, cache and configuration path existed in source. Software-renderer qualification did not establish GPU pixels, damage handling, cache behavior or frame cost. [docs]

## Learnings

- A layout model and command parser do not establish the complete interactive contract. [docs]
- Compatibility aliases preserve command vocabulary while the donor layout model still determines behavior. [docs]
- Software rendering does not qualify a GPU-specific effects path. [docs]

## Ending and carry-forward

Work stopped for reasons outside this library's scope; the technical state at that point was a software-renderer-qualified fork with incomplete tabbed-column interaction and unverified GPU effects. [docs]

The [sway-next family page](../04-sway-family/sway-next.md) preserves the detailed state. Related mechanisms appear under [scrolling layout](../06-features/layout/scrolling-layout.md), [overview](../06-features/workspaces/overview.md) and [backdrop blur](../06-features/effects/backdrop-blur.md).

## Sources

- S-abb5938fade4 · 2026-11-08 · Recorded compositor feature state.
- S-5e377e6e9b90 · 2026-11-08 · Preserved compositor state and ending.
- S-aecf1c64f5c9 · 2026-10-23 · Software-renderer strip painting, interaction and persistence qualification.
