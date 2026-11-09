---
id: 01-chromium-lineage/cade
title: "CADE: Ash and Aura on generic Linux DRM"
kind: era
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-90efe842b815, S-7f4e4b3b464a, S-8b05eb1370ff, S-a5d1670abc79, S-a16cc8bdfb57, S-0f934972e3ae, S-9eeaa5387ca1, S-fd16ec144654, S-703c0d408b29]
---

# CADE: Ash and Aura on generic Linux DRM

CADE explored Chromium as the desktop compositor: Ash supplied window-management machinery, Ozone supplied DRM presentation and Exo hosted Wayland applications. [docs]

## Dates and demonstrated scope

- The June 10 VM probe recorded DRM master, software rendering and an accepted Exo client. Its final report retracts scanout: the active display retained its previous framebuffer. Per-frame client drawing, end-to-end input and comparative performance remained unqualified. [docs]
- The reconstruction locates an early maximal compositor discussion on 2026-04-11 and the keep-Ash investigation in June. [docs]
- A 2026-06-28 result records a ChromeOS-target build booting deeply on fake service clients, authenticating DRM and initializing graphics in a seated Linux session. [docs]
- The same result records a transition from bundled minigbm to system Mesa GBM. Its final retained state still required a real display sink; stable GPU initialization alone did not prove scanout or Exo application presentation. [docs]
- June 29 primary records then report physical-display modesetting, Ash rendering and Exo clients. Runtime OS gates and initialization order had prevented display configuration despite stable graphics initialization. [docs]
- A later June 29 record reports native browser typing and wheel scrolling after targeted input changes. A GTK client exposed an unresolved buffer-import failure that starved rendering until the client was killed. These are dated observations from that setup, not broad client compatibility results. [docs]

## Mechanisms and handoff

The [Ozone chapter](mechanisms/ozone-platform-work.md) keeps platform plumbing distinct from shell policy. The [Aura/Ash/Exo map](../02-aura-ash-exo/README.md) separates available engine machinery from ChromeOS-gated code. [docs]

> Era and its assumptions: CADE, which assumed the project would port Ash and own the compositor. For Domicile this probably adds little: Domicile already uses Chromium with Smithay delivery and a web shell. Recorded because successful build and graphics initialization do not qualify the service and protocol integration above them.


## Where it touches Domicile

[Window compositing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md) defines Domicile's application-layer path; it does not imply that Ash or Exo is compiled into the Linux engine. [docs]


## Sources

- S-a16cc8bdfb57 · 2026-06-28 · Generic Linux Ash and graphics bring-up results
- S-0f934972e3ae · 2026-06-29 · Ash build and display qualification boundary
- S-9eeaa5387ca1 · 2026-06-29 · CUBS compositor and toolkit axes
- S-fd16ec144654 · 2026-10-28 · Chromium and Aura lineage reconstruction
- S-a5d1670abc79 · 2026-06-10 · VM DRM and Exo shared-memory client result
- S-7f4e4b3b464a · 2026-06-29 · Physical-display modeset progression and runtime gates
- S-8b05eb1370ff · 2026-06-29 · Interactive Ash, Exo clients and remaining integration limits
- S-90efe842b815 · 2026-06-10 · VM compositor probe and final scanout retraction
- S-703c0d408b29 · 2026-11-09 pin check · Domicile native surface composition and presentation boundaries
