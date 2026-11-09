---
id: "01-chromium-lineage/README"
title: "Chromium desktop lineage"
kind: "collection"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile"]
sources: [S-fd16ec144654, S-c01d4f89b475, S-e77ffff40807, S-612a99e2b687, S-328370bdeca1, S-541270a86f10, S-703c0d408b29, S-e4ff18df1f16, S-fc035ad1863f, S-1a653077d862, S-514bff6c69a0]
---

# Chromium desktop lineage

This chapter records earlier investigations and their limits without prescribing an implementation sequence.

The eras explore two independent axes: who owns the desktop scene, and whether shell controls are native or web-rendered. A shared name does not establish a shared binary or runtime result. Architecture chapters also overlap in time. [docs]

## Era table

| Era | Dates | What it was | What it demonstrated | Handed forward | Where the piece lives at the Domicile pin |
|---|---|---|---|---|---|
| [Headwaters: a web shell with a typed namespace](headwaters.md) | December 2025; March 2026 consolidation | Web shell in a native host | Typed state, operations and subscriptions [docs] | Page/host contract | [Typed SDK and host routing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/system-calls.ts#L5) |
| [quickshellX / webshell: web content under niri](quickshellx-webshell.md) | March 2026 | QtWebEngine over niri | Transparent panels and system bridges; host clipping limit [docs] | Surface geometry and typed services | [One desktop page](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md#L3) |
| [REVOLUTION: a purpose-built Chromium embedder](revolution.md) | March–April 2026 | Purpose-built Content API embedder | Surface-role and browser-host investigation [docs] | Ozone roles and fork shape | [Engine overlay](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md) |
| [COWL: Chromium as a layer-shell client](cowl.md) | March–April 2026 | Content API layer-shell client | Compilation and responsive diagnostics; rendering/service gaps retained [docs] | Typed Mojo and buffer submission | [Native surfaces in the page](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L228) |
| [CADE: Ash and Aura on generic Linux DRM](cade.md) | April–June 2026; June 28 bring-up, June 29 display/input | Ash/Aura compositor with Exo | DRM ownership and Exo liveness; later display/input qualification [docs] | Scene ownership and service gates | [External Rust delivery](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md#L352) |
| [COWL revived and the patch campaign](cowl-revived.md) | June 2026 | Browser-and-shell client and patch campaign | Surface roles, opacity and workspace interaction [docs] | Ozone host and focus seams | [Engine surface bridge](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md#L295) |
| [CUBS: one browser process, a native Views shell](cubs.md) | June 2026 | Tier-2 fork with Views shell daemon | Native Views interaction on a layer-shell host [docs] | Toolkit versus compositor separation | [Manganese shell composition](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md) |
| [The July 2026 native-shell floor](july-native-shell-floor.md) | July 2026 | CUBS native-shell continuation on niri | Build recovery and virtual-output controls; physical capture unqualified [docs] | Build graph and widget invariants | [Engine packaging](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/README.md) |
| [views-shell and the cowl:// study](views-shell-and-cowl-scheme.md) | October 2026 | Standalone Views client and system-page study | Headless build and interaction; separate source-only scheme study [docs] | Surface role, origin and authorization | [Shell page and SDK](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md) |
| [A 2026-10-28 Chromium/Aura lineage report](october-aura-report.md) | 2026-10-28 | Chromium/Aura lineage reconstruction | Chronology and evidence-boundary comparison [docs] | Mechanism inventory | [Feature and availability map](../02-aura-ash-exo/README.md) |
| [Domicile: the shipped form recognized in November](domicile-recognition.md) | 2026-11-04 | Recognition of Domicile as the shipped form | Structural correspondence with Domicile [docs] | Closed prior investigation | [Current architecture](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ARCHITECTURE.md) |

The Domicile column is a reading map into [the pinned architecture](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ARCHITECTURE.md), [engine fork](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md), [one page for the desk](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md) and [binary supervision](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/THE-DOMICILE-BINARY.md). It is not a claim that the older implementations were imported. [inferred]

## Mechanism chapters

- [Chromium builds on Nix](mechanisms/chromium-on-nix.md)
- [Layer-shell client and compositor ownership](mechanisms/layer-shell-client-and-compositor.md)
- [Ozone platform work](mechanisms/ozone-platform-work.md)
- [Tier-2 forks and rebase cost](mechanisms/tier-two-fork-rebase-cost.md)
- [The typed page/host seam](mechanisms/typed-page-host-seam.md)
- [The unified-binary insight](mechanisms/unified-binary.md)
- [Views on layer-shell](mechanisms/views-on-layer-shell.md)
- [The web-shell surface ceiling](mechanisms/web-shell-ceiling.md)

## Sources

- S-fd16ec144654 · 2026-10-28 · Chromium and Aura lineage reconstruction
- S-c01d4f89b475 · 2026-11-04 · Earlier compositor ideas compared with Domicile
- Each era page supplies its own primary records and evidence limits.
- S-e77ffff40807 · 2026-11-09 pin check · Pinned shell authoring contract
- S-612a99e2b687 · 2026-11-09 pin check · Pinned SDK typed desktop service declarations
- S-328370bdeca1 · 2026-11-09 pin check · Pinned engine packaging and dependency contract
- S-541270a86f10 · 2026-11-09 pin check · Pinned shell reuse and component boundary
- S-703c0d408b29 · 2026-11-09 pin check · Domicile native surface composition and presentation boundaries
- S-e4ff18df1f16 · 2026-11-09 pin check · Domicile desktop-page and presenter architecture
- S-fc035ad1863f · 2026-11-09 pin check · Domicile launcher, process supervision and recovery boundaries
- S-1a653077d862 · 2026-11-09 pin check · Pinned Domicile architecture and process boundary
- S-514bff6c69a0 · 2026-11-09 pin check · Domicile engine integration and patch discipline
