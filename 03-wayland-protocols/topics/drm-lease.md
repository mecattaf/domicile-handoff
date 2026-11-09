---
id: "protocols/topics/drm-lease"
title: "DRM lease and display ownership"
kind: "topic"
layers: ["engine:drm", "compositor", "protocol", "external"]
domicile_status: "absent"
domicile_refs: []
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: "baseline"
evidence_mix: "mostly-source"
boundary_note: false
bucket: "domicile"
tags: ["layer:protocol", "evidence:source"]
sources: [S-1951a846e891, S-3f524db52413, S-7f8302736ae8, S-e8cd22f8b451, S-ef86748c2a79, S-f152f60cb06a]
family: protocols
---

# DRM lease and display ownership

A DRM lease gives a client direct use of selected display resources, commonly for a dedicated headset. It differs from rendering a fullscreen window on an output. [docs]

## Availability and ownership

| Layer | State at the pin |
|---|---|
| Smithay | Contains a DRM lease handler and version-1 lease-device global implementation behind the `backend_drm` feature gate |
| Domicile compositor | No lease state, handler delegation or global in the complete executable-module registration search |
| Domicile tty engine | logind grants the DRM card; the engine receives the descriptor and manages display operation |

Smithay contains the handler source behind `backend_drm`. The compositor manifest disables Smithay defaults and selects `wayland_frontend`, `backend_egl` and `renderer_gl`; it does not select that gate. Crate-wide feature unification is not audited here. [source] [Dependency features](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/Cargo.toml#L79) · [Module gates](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/mod.rs#L55)

Handler source presence does not establish protocol registration. [pinned Smithay handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/drm_lease/mod.rs#L303) [source]

The engine's documented DRM ownership is an additional integration boundary: the Wayland compositor renders on a render node while the engine handles display resources received from logind. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/TTY-SESSION.md#L25), [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/A-DESKTOP-ON-A-TTY.md#L270) [docs]

The map records DRM lease as absent at the pin. A fullscreen client, a virtual monitor, a gamma client and a leased connector represent different control and lifetime contracts. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L817) [source] [inferred]

Related features: [output profiles and display power](../../06-features/outputs/output-management-and-power.md), [fullscreen display requests](../../06-features/outputs/fullscreen-display-choice.md), [device access and tty session](../../06-features/seats/libseat-and-session.md).

## Sources

- S-e8cd22f8b451 · 2026-11-09 · Compositor dependency feature selection.
- S-ef86748c2a79 · 2026-11-09 · Wayland handler module feature gates.

- S-1951a846e891 · 2026-11-09 · Direct-session architecture and native device ownership.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-7f8302736ae8 · 2026-11-09 · Smithay DRM lease handler at the dependency pin.
- S-f152f60cb06a · 2026-11-09 · Logind card, input and console lifetime contract.
