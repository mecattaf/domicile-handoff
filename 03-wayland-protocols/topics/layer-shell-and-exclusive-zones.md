---
id: "protocols/topics/layer-shell-and-exclusive-zones"
title: "Layer-shell and exclusive zones"
kind: "topic"
layers: ["engine:blink", "compositor", "protocol", "sdk", "manganese", "any-shell"]
domicile_status: "by-design-elsewhere"
domicile_refs: []
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: "baseline"
evidence_mix: "mostly-source"
boundary_note: false
bucket: "domicile"
tags: ["layer:protocol", "evidence:source"]
sources: [S-1951a846e891, S-3f524db52413, S-e4ff18df1f16]
family: protocols
---

# Layer-shell and exclusive zones

Layer-shell lets an independent Wayland client request an anchored desktop surface, a stacking layer, keyboard-interactivity policy and reserved output space. Domicile places desktop chrome in its spanning shell page. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md#L1) [docs]

## Mechanism and boundary

| Mechanism | What it carries | Domicile at the pin |
|---|---|---|
| `wlr-layer-shell-unstable-v1` | Anchors, margins, layer, keyboard behavior and exclusive zone | No layer-shell global, state or delegate in the complete executable compositor registration scan |
| Shell page layout | Panel space, overlays and placement of native-window elements | Shell/Manganese responsibility within the one-page desktop |
| Native client toplevel | Ordinary application window role | xdg shell remains the native-window route |

The registration inspection establishes protocol absence. The upstream page architecture assigns chrome placement to the desktop page. Neither fact proves that an arbitrary external layer-shell client can use a shell page as its transport. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L817), [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md#L1) [source] [inferred]

An exclusive zone describes reserved work area. Anchoring an element on an output, drawing it over another surface and deciding whether it can take keyboard focus are related but distinct shell behaviors. [docs]

The feature crosswalk keeps the source protocol contract separate from the shell mechanism that supplies a similar desktop role. [inferred]

Related feature: [panels, overlays and reserved space](../../06-features/shell-surfaces/layer-shell-equivalents.md).

## Sources

- S-e4ff18df1f16 · 2026-11-09 · Spanning desktop page and single-seat architecture.

- S-1951a846e891 · 2026-11-09 · Direct-session architecture and native device ownership.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
