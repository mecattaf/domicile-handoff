---
id: 01-chromium-lineage/mechanisms/views-on-layer-shell
title: "Views on layer-shell"
kind: mechanism
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-99f03266e9b0, S-ed6c0b655bea, S-e3eb603fbdcb, S-e68d7932aa6c, S-80f21569978a, S-5d886dcdc3b2, S-8f764afc6722, S-1c58e1373c0b, S-89151c2daaf3, S-34997c703058, S-e77ffff40807]
---

# Views on layer-shell

The 2026-06-30 result reports a native Views Widget attached to a layer-shell-backed Aura root. The surface painted and received keyboard input without a WebContents renderer. [docs]

## Eras and mechanism

COWL supplied the Ozone layer-shell window. CUBS attached NativeWidgetAura and a Views tree to that host. Input then crossed both Wayland keyboard delivery and Views focus routing. [docs]

> Era and its assumptions: CUBS, which assumed native shell controls would run inside the browser process under niri. For Domicile this probably adds little: Domicile already renders its shell in a web page. Recorded because window hosting and keyboard focus are separate seams even when the toolkit already paints.


## What was observed and what was inferred

- The result records arrow-key selection, Enter logging and Escape dismissal. It does not claim the demonstration rows launched applications. [docs]
- The June 30 exploration records a separate dismissal defect: hiding the inner Widget left its fullscreen host surface mapped. View visibility and host-surface lifetime remained different operations. [docs]
- A companion comparison records a renderer-free Views surface in one component-build sample. Exact memory or idle differences do not establish a general toolkit ranking. [docs]
- July secondary widgets did not automatically inherit the earlier BrowserWidget activation path. Events could reach the Wayland surface while missing the focused Views root. [docs]
- The October architecture records a dedicated in-process Viz context factory, independent of Content, replacing an earlier test-support startup. The October 2 Chromium 154 account forwards the parent `LocalSurfaceId` into Viz before configure acknowledgment. Both software and SwiftShader modes attached buffers in a headless setup; physical-GPU presentation remained unqualified. [docs]
- An October headless gallery control stayed alive without attaching a buffer. Its account traced discarded frames to mismatched `LocalSurfaceId` allocation in the test frame-sink arrangement. [docs]
- The October headless recheck recorded layer-surface drawing and popup children. An invocation without a shell-surface role exited without drawing. [docs]
- That architecture also records a geometry gap: changing Widget bounds did not carry the layer-surface margin update, so notification restacking retained its earlier placement. Toolkit geometry and Wayland host placement remained separate responsibilities. [docs]
- A separate October 2 assembled report exposed a GPU render node in one headless run while the client still composited in software. Device availability did not establish GPU client composition or physical presentation. [docs]
- The inference is that proof of one widget-host arrangement does not qualify every secondary surface arrangement. [inferred]

## Where it touches Domicile

The current shell boundary is [the page authoring API](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md). This native-host chapter records focus and lifetime lessons. [docs]


## Sources

- S-99f03266e9b0 · 2026-10-01 · Headless gallery frame-ID coordination and missing buffer attachment
- S-ed6c0b655bea · 2026-10-02 · Standalone layer-surface hosting, keyboard input and popup verification
- S-5d886dcdc3b2 · 2026-06-30 · Native Views layer-shell interaction result
- S-8f764afc6722 · 2026-06-30 · Single-sample native and web shell comparison
- S-1c58e1373c0b · 2026-07-16 · Native shell recovery and live widget limits
- S-e68d7932aa6c · 2026-10-02 snapshot bound · Standalone Viz hosting and layer-surface margin debt
- S-80f21569978a · 2026-10-02 · Standalone Views build and headless interaction result
- S-e3eb603fbdcb · 2026-06-30 · Native host visibility, surface lifetime and unresolved shell arrangements
- S-89151c2daaf3 · 2026-10-02 · Production Viz hosting and headless software-rendering qualification
- S-34997c703058 · 2026-10-02 · Assembled headless shell and render-node versus client-compositing boundary
- S-e77ffff40807 · 2026-11-09 pin check · Pinned shell authoring contract
