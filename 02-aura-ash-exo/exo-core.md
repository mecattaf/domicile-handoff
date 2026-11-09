---
id: "02-aura-ash-exo/exo-core"
title: "Exo core"
kind: "collection"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile", "evidence:source"]
sources: [S-1f554a1713a5, S-255b69aec99d, S-4111d1fe471f, S-5cfd5323e726, S-6299b9bca7d0, S-73bd30934d13, S-aafc0349dee2, S-c153d333f325, S-da9b171d79ff, S-514bff6c69a0]
---

# Exo core

Exo models surfaces, buffers, subsurfaces, shell roles and security delegation inside Chromium. The target remains ChromeOS-gated even for classes with little direct Ash coupling. [docs]

## What Chromium already has

The [row data](rows/aura-lift.tsv) holds 43 rows for this beat: ADAPT 8, FRESH 13, LIFT 22. These are historical source verdicts. [docs]

Donor availability in those rows: `chromeos-gated` 43. Zero Ash references in a file do not bypass its target build gate. [docs]

- Exo remains a ChromeOS-gated static library; low Ash coupling in an individual file does not make the Exo target available to a Linux engine build. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/BUILD.gn#L13).
- Exo DataDevice handles clipboard selection separately from drag entry, update and drop callbacks; portability still inherits the Exo build gate. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/data_device.cc#L80), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/data_device.cc#L87).
- WMHelper remains a concrete singleton with direct Ash getters and a ChromeOS power-client observer base; its facade is not a replaceable neutral backend by declaration alone. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wm_helper.h#L58), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wm_helper.cc#L114).
- The Exo DMA BUF factory requests zero copy and overlay candidacy for imported buffers; these are selected flags rather than measured scanout or performance results. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/display.cc#L108).

## Domicile at the pin

- `engine:browser and compositor`: Domicile explicitly keeps its Rust compositor external to Chromium. The browser ports the useful Exo buffer-import pattern because it owns aura::Env; the producer submits frames directly. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md#L295).

## Frame-lifetime correspondence

| Boundary | Exo holder at the engine pin | Domicile native producer at the desktop pin |
|---|---|---|
| Acknowledgment | [Frame-sink ACK callback](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/layer_tree_frame_sink_holder.cc#L188) | [Native frame ACK event](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L405) |
| Resource return | [Returned-resource callback](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/layer_tree_frame_sink_holder.cc#L159) | [Native resource-return event](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L447) |
| Presentation | [Presentation callback](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/layer_tree_frame_sink_holder.cc#L212) | [Native presentation event](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L428) |

The sources expose comparable boundaries. Callback ownership, transport and client-destruction paths remain implementation-specific; the correspondence does not establish API or runtime equivalence. The Exo side retains its ChromeOS build gate. [inferred]

## Boundary

Server-side semantics can inform Smithay delivery without importing the Exo target. Buffer import, surface state, capability policy and scene ownership are separate seams. [inferred]

## Feature links

- [engine-mechanisms/gpu-and-render-backends](../06-features/engine-mechanisms/gpu-and-render-backends.md)
- [shell-surfaces/popup-and-popover-boundaries](../06-features/shell-surfaces/popup-and-popover-boundaries.md)

## Sources

- S-73bd30934d13 · 2026-07-18 · Chromium exo core source and equivalence inventory
- S-1f554a1713a5 · 2026-07-18 · Lift-map evaluation and corrections
- S-255b69aec99d · 2026-11-09 · Pinned donor interface inspection
- S-4111d1fe471f · 2026-11-09 · Pinned donor interface inspection
- S-5cfd5323e726 · 2026-11-09 · Pinned donor interface inspection
- S-6299b9bca7d0 · 2026-11-09 · Pinned donor interface inspection
- S-aafc0349dee2 · 2026-11-09 · Pinned donor interface inspection
- S-c153d333f325 · 2026-11-09 · Pinned donor interface inspection
- S-da9b171d79ff · 2026-11-09 · Pinned donor interface inspection
- S-514bff6c69a0 · 2026-11-09 pin check · Domicile engine integration and patch discipline
