---
id: 01-chromium-lineage/cowl
title: "COWL: Chromium as a layer-shell client"
kind: era
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-966ffabac4e2, S-41ce2fd9abfe, S-56fb722c4f08, S-8b956f5955ba, S-35edbbab9f4e, S-1d5f9c76f3df, S-c6f41daa6909, S-dfd7b6e6962d, S-6f807e7785c4, S-fd16ec144654, S-1a653077d862]
---

# COWL: Chromium as a layer-shell client

COWL was a Chromium Content API embedder and later a browser-plus-shell fork. Its shell surfaces were Wayland layer-shell clients under niri. It did not own desktop compositing in this shape. [docs]

## Dates and demonstrated scope

- March 2026 sources describe a Content API embedder; April studies also discuss retaining the browser layer. The documents record changing build shapes rather than one immutable implementation. [docs]
- The retained June runtime branch records web shell surfaces in patched Ozone, with niri handling geometry. [docs]
- The March scaffold evaluation records renderer-startup and surface-control gaps. The April architecture record treats earlier source as reference material needing validation; the integration audit likewise retains build and lifecycle gaps. These code reads do not establish a complete runtime. [docs]
- A retained Chromium 148 handoff reports compilation while leaving the initial launch and protocol execution checks outstanding. The compilation report therefore supplies no boot qualification. [docs]
- A separate March 30 benchmark account reports running processes and responsive JavaScript evaluation through DevTools. Command and D-Bus service calls crashed the renderer. This establishes a reported process-runtime tier, without visible shell or complete protocol qualification. [docs]
- COWL was not a CEF off-screen-rendering implementation. The September correction explicitly distinguishes the built native embedder from the surveyed CEF route. [docs]

## Mechanisms and handoff

A privileged page called typed Mojo services. Pixel transport used ordinary Wayland buffer submission through the patched Ozone backend. Compositor queries used niri's own IPC; these were three different paths. [docs]

> Era and its assumptions: COWL, which assumed a browser process created multiple shell surfaces under an external compositor. For Domicile this probably adds little: Domicile already places native applications in its own desktop page. Recorded because control messages and pixel transport have distinct ownership and lifetime rules.


## Where it touches Domicile

[Layer-shell client and compositor ownership](mechanisms/layer-shell-client-and-compositor.md) contrasts this boundary with [Domicile architecture](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ARCHITECTURE.md). [docs]


## Sources

- S-c6f41daa6909 · 2026-07-03 · Content-dated spring shell chronology
- S-dfd7b6e6962d · 2026-06-16 · Browser and shell client revival
- S-6f807e7785c4 · 2026-09-27 · Embedder evidence corrections and comparison
- S-fd16ec144654 · 2026-10-28 · Chromium and Aura lineage reconstruction
- S-35edbbab9f4e · 2026-04-05 · Browser-and-shell client architecture and validation limits
- S-1d5f9c76f3df · 2026-04-07 · COWL integration audit and unresolved lifecycle/binding gaps
- S-56fb722c4f08 · 2026-03-29 · Early scaffold compilation and unresolved rendering/control gaps
- S-8b956f5955ba · 2026-06-27 · Chromium client surfaces and keyboard qualification
- S-41ce2fd9abfe · 2026-05-31 month upper bound · Chromium 148 compilation and first-launch qualification boundary
- S-966ffabac4e2 · 2026-03-30 · Chromium 148 process diagnostics and service-call crashes
- S-1a653077d862 · 2026-11-09 pin check · Pinned Domicile architecture and process boundary
