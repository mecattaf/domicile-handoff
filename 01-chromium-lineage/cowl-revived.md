---
id: 01-chromium-lineage/cowl-revived
title: "COWL revived and the patch campaign"
kind: era
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-b73a4b36aa7b, S-8b956f5955ba, S-dfd7b6e6962d, S-81c276c60385, S-5d886dcdc3b2, S-fd16ec144654, S-514bff6c69a0, S-d32f2f2a41d2]
---

# COWL revived and the patch campaign

The 2026-06-16 revival restored the browser-and-shell client boundary under niri. The June 26–27 patch campaign made the surface path observable and exposed integration details hidden by a successful compile. [docs]

## Revival

The notes separate Mojo control, Wayland pixel submission and niri window/workspace IPC. Retaining the browser became an explicit part of the revived shape. [docs]

## Patch campaign

- The June 27 primary report records layer-shell surfaces and a workspace switcher whose collapse resized the actual surface and exclusive zone. It distinguishes CDP typing confirmation from the still-open physical-keyboard path. [docs]
- The lineage records a transparent-surface problem caused by an opaque-region declaration in Ozone. Correct alpha content was insufficient while the compositor was told the entire surface was opaque. [docs]
- A June 26 progress addendum reports live surface content after rebuilding with assertions disabled. Earlier geometry-only success and blocked service initialization were distinct stages; several D-Bus services stayed disabled. [docs]
- The browser fork's build and patch surface remained a separate maintenance concern. Quoted rebase budgets were estimates, not measured roll costs. [docs]

## Handoff

The layer-shell Ozone path became the foundation for the June 30 Views weld. [docs]

> Era and its assumptions: COWL revived, which assumed the browser and shell shared a process tree while niri owned placement. For Domicile this probably adds little: Domicile already owns the desktop scene through the page and engine. Recorded because protocol role, transparency declarations and keyboard ownership remain separate bring-up checks.


## Where it touches Domicile

[Views on layer-shell](mechanisms/views-on-layer-shell.md) records the historical weld; [the engine boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md) describes the current one. [docs]


## Sources

- S-b73a4b36aa7b · 2026-06-27 · Surface opacity declaration and browser patch record
- S-dfd7b6e6962d · 2026-06-16 · Browser and shell client revival
- S-81c276c60385 · 2026-06-29 · Spring and June compositor lineage reconstruction
- S-5d886dcdc3b2 · 2026-06-30 · Native Views layer-shell interaction result
- S-fd16ec144654 · 2026-10-28 · Chromium and Aura lineage reconstruction
- S-8b956f5955ba · 2026-06-27 · Chromium client surfaces, workspace collapse and keyboard qualification
- S-514bff6c69a0 · 2026-11-09 pin check · Domicile engine integration and patch discipline
- S-d32f2f2a41d2 · 2026-06-26 · Mapped-surface, page rendering and native-service startup boundaries
