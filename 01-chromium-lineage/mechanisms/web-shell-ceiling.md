---
id: 01-chromium-lineage/mechanisms/web-shell-ceiling
title: "The web-shell surface ceiling"
kind: mechanism
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-06b83f1982b4, S-ab96089cfecd, S-c6f41daa6909, S-dfd7b6e6962d, S-9eeaa5387ca1, S-fd16ec144654, S-e4ff18df1f16, S-3abebbad32a3]
---

# The web-shell surface ceiling

The earlier web shell was clipped to each host surface. A dropdown could not paint outside a narrow panel unless the host allocated another surface or changed the surface geometry. [docs]

## How the eras answered it

| Era | Recorded answer |
|---|---|
| quickshellX / webshell | Qt host supplied panel surfaces; the web view remained within its item bounds |
| REVOLUTION / COWL | Own the embedder and create additional Wayland shell surfaces through Ozone |
| CADE | Own the application scene through Aura, Ash and Exo |
| CUBS | Put stable shell controls in native Views and retain an external compositor |
| Domicile | One desktop page contains native application elements |

Historical rows describe the notes; the Domicile row follows the pinned architecture. [docs]

> Era and its assumptions: the panel web-shell eras, which assumed every panel was a separately clipped native surface. For Domicile this probably adds little: Domicile already places the desk in one page spanning displays. Recorded because a clipping limit belongs to the owning surface boundary rather than to HTML in isolation.


## Evidence and limits

The recorded clipping limit concerned native surface allocation and focus beyond the panel. It does not establish that HTML lacks popovers, menus or transforms. [inferred]

## Where it touches Domicile

[One page for the desk](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md) describes one page layout and frame spanning outputs, with per-display presenters. The narrow-panel escape problem therefore dissolves at the page boundary; clipping, stacking and native popup membership still have their own rules. [inferred]

[Stacking parity](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/STACKING-PARITY.md) is the current reference for those rules. [docs]


## Sources

- S-c6f41daa6909 · 2026-07-03 · Content-dated spring shell chronology
- S-dfd7b6e6962d · 2026-06-16 · Browser and shell client revival
- S-9eeaa5387ca1 · 2026-06-29 · CUBS compositor and toolkit axes
- S-fd16ec144654 · 2026-10-28 · Chromium and Aura lineage reconstruction
- S-06b83f1982b4 · 2026-03-25 · Web shell and system-service bridge result
- S-ab96089cfecd · 2026-03-26 · Host-surface clipping and dropdown limits
- S-e4ff18df1f16 · 2026-11-09 pin check · Domicile desktop-page and presenter architecture
- S-3abebbad32a3 · 2026-11-09 pin check · Pinned stacking and transient-window contract
