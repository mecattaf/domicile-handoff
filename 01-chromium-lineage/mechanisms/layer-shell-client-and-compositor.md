---
id: 01-chromium-lineage/mechanisms/layer-shell-client-and-compositor
title: "Layer-shell client and compositor ownership"
kind: mechanism
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-90efe842b815, S-dfd7b6e6962d, S-9eeaa5387ca1, S-a16cc8bdfb57, S-5d886dcdc3b2, S-ade47bf425f2, S-140bdec3f097, S-e4ff18df1f16, S-703c0d408b29]
---

# Layer-shell client and compositor ownership

A layer-shell client submits its own pixels and requests anchoring, layer and exclusive space. A compositor owns placement, focus delivery, application visibility and final presentation. The Chromium eras explored both boundaries. [docs]

## Eras

| Era | Surface and scene ownership |
|---|---|
| quickshellX / webshell | Qt host created panels; niri owned the scene |
| COWL | Patched Ozone created layer-shell surfaces; niri owned the scene |
| CADE | Ash/Aura owned the scene; Exo served Wayland clients |
| CUBS design argument | Views supplied shell surfaces; niri owned the scene |

The table is the notes' architectural classification, not a current availability claim. [docs]

The April 10 studies preserved two proposals: keep COWL as a client of a separately built compositor, or move scene ownership into Chromium/Aura/Viz. These record alternative designs, without establishing which proposal ran. [docs]

> Era and its assumptions: COWL and CADE, which assumed surface creation and compositor ownership were alternative fork goals. For Domicile this probably adds little: Domicile already owns a desktop page backed by native application surface layers. Recorded because a shell client cannot acquire scene authority merely by using the compositor toolkit.


## Evidence boundary

The June 10 compositor probe retained DRM ownership and Exo client liveness, while its corrected report retracts scanout. The June Views result reports painting and keyboard focus on a layer-shell surface. The June 28 headless Ash result records build and GPU initialization with physical presentation pending. They qualify different mechanisms. [docs]

## Where it touches Domicile

[One page for the desk](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md) places shell surfaces in a page spanning monitors. [Window compositing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md) documents native surfaces inside that scene. Translating old exclusive-zone behavior to page layout is a semantic comparison rather than a binary code lift. [inferred]


## Sources

- S-dfd7b6e6962d · 2026-06-16 · Browser and shell client revival
- S-9eeaa5387ca1 · 2026-06-29 · CUBS compositor and toolkit axes
- S-a16cc8bdfb57 · 2026-06-28 · Generic Linux Ash and graphics bring-up results
- S-5d886dcdc3b2 · 2026-06-30 · Native Views layer-shell interaction result
- S-ade47bf425f2 · 2026-04-10 · Separate-compositor and client interface proposal
- S-140bdec3f097 · 2026-04-10 · Chromium-owned compositor alternative
- S-90efe842b815 · 2026-06-10 · VM compositor probe and corrected scanout boundary
- S-e4ff18df1f16 · 2026-11-09 pin check · Domicile desktop-page and presenter architecture
- S-703c0d408b29 · 2026-11-09 pin check · Domicile native surface composition and presentation boundaries
