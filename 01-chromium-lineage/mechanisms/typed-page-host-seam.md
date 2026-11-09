---
id: 01-chromium-lineage/mechanisms/typed-page-host-seam
title: "The typed page/host seam"
kind: mechanism
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-ed6c0b655bea, S-966ffabac4e2, S-b060431106d8, S-daf04bfaa40d, S-64c374e9089b, S-1d5f9c76f3df, S-dfe38f275826, S-dfd7b6e6962d, S-9eeaa5387ca1, S-514bff6c69a0, S-e77ffff40807]
---

# The typed page/host seam

The historical seam exposed system state, asynchronous operations and subscriptions to a web shell while native code owned privileged resources. It moved from QtWebChannel to Mojo without requiring the same renderer framework. [docs]

## Eras

Headwaters and quickshellX used a typed namespace over a Qt host. COWL used renderer bindings and typed Mojo services. CUBS retained Mojo for service/data transport while moving native shell painting into Views. [docs]

> Era and its assumptions: the typed web-shell eras, which assumed a privileged browser host supplied all system capabilities. For Domicile this probably adds little: Domicile already provides its own SDK, host routing and compositor interfaces. Recorded because typed data and explicit lifetimes survived transport and renderer changes.


## What the notes actually established

- The contract separated properties, operations and event subscriptions. Data crossing the seam was serializable rather than a live native object graph. [docs]
- A March 24 static integration audit recorded mismatches between frontend expectations and native QtWebChannel registrations and signal names. Mock data covered interfaces absent from those registrations; it did not establish native service delivery. [docs]
- The March 25 account then distinguished active interfaces from stubs and skipped hydration for the latter. An empty namespace remained compatible with an unimplemented service. [docs]
- The COWL revival explicitly distinguished Mojo control from Wayland pixels and niri IPC. [docs]
- The April integration audit treated renderer-side binding exposure and browser-side authorization separately, and recorded an unresolved browser-side check. A later service implementation was tied to frame-host lifetime; that addresses a lifecycle class independently of authorization. [docs]
- A March 29 static audit recorded matching renderer/Mojo methods alongside incompatible C++ host signatures and missing host methods. An aligned interface catalog did not establish a compiling end-to-end host chain. [docs]
- The March 30 diagnostic account reports responsive DevTools JavaScript evaluation while command and D-Bus service calls crashed the renderer. Diagnostic reachability and native service execution remained separate qualifications. [docs]
- The October native-shell verification found statically advertised adapter capabilities without command, event or request paths. Capability listing and registry admission did not establish execution support. [docs]
- The historical contracts supply no Domicile IPC latency measurement. [docs]

## Where it touches Domicile

[Engine-fork architecture](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md) documents the page/browser/compositor interfaces. [Shell authoring](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md) is the current page-facing contract. The old namespace is a closed design chapter. [docs]

The older contracts treat authorization, object lifetime and serializable data as distinct concerns from renderer framework choice. [inferred]


## Sources

- S-ed6c0b655bea · 2026-10-02 · Native-shell capability advertisement and executable-path verification
- S-dfe38f275826 · 2026-03-21 · Consolidated December web-shell contract
- S-dfd7b6e6962d · 2026-06-16 · Browser and shell client revival
- S-9eeaa5387ca1 · 2026-06-29 · CUBS compositor and toolkit axes
- S-1d5f9c76f3df · 2026-04-07 · COWL frame lifetime, interface binding and platform-role audit
- S-64c374e9089b · 2026-03-29 · Static renderer, Mojo and host-interface audit
- S-514bff6c69a0 · 2026-11-09 pin check · Domicile engine integration and patch discipline
- S-daf04bfaa40d · 2026-03-24 · Static frontend, native-object registration and signal integration audit
- S-b060431106d8 · 2026-03-25 · Native bridge integration symptoms and explicit interface stubs
- S-966ffabac4e2 · 2026-03-30 · JavaScript diagnostics and native-service execution limits
- S-e77ffff40807 · 2026-11-09 pin check · Pinned shell authoring contract
