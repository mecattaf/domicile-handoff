---
id: "02-aura-ash-exo/ash-splitview-cycle"
title: "Ash split view and window cycling"
kind: "collection"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile", "evidence:source"]
sources: [S-1f554a1713a5, S-5a284826e67f, S-b5dd4dc770f0, S-a59107f9d164]
---

# Ash split view and window cycling

Split view constrains window bounds. Cycling previews candidates and commits or cancels activation through a separate controller. [docs]

## What Chromium already has

The [row data](rows/aura-lift.tsv) holds 14 rows for this beat: ADAPT 12, FRESH 2. These are historical source verdicts. [docs]

Donor availability in those rows: `chromeos-gated` 14. Zero Ash references in a file do not bypass its target build gate. [docs]

- MRU construction still consults Ash focus, floating-window desk membership and root-window accessors; the corrected historical ADAPT verdict remains justified. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/mru_window_tracker.cc#L76), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/mru_window_tracker.cc#L152).

## Domicile at the pin

- `manganese and sdk`: Window state requests are separate from tiled-edge state delivery; shell keybindings are independent of the native client request. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/window-state.ts#L20).

## Boundary

A candidate list can be rendered in the shell while completion, cancellation and MRU policy retain their own state. Split layout and cycle presentation are separate comparisons. [inferred]

## Feature links

- [aura-wm/split-view-and-cycle](../06-features/aura-wm/split-view-and-cycle.md)

## Sources

- S-b5dd4dc770f0 · 2026-07-18 · Chromium ash split view and window cycling source and equivalence inventory
- S-1f554a1713a5 · 2026-07-18 · Lift-map evaluation and corrections
- S-5a284826e67f · 2026-11-09 · Pinned donor interface inspection
- S-a59107f9d164 · 2026-11-09 pin check · Pinned native-window state and request enumeration
