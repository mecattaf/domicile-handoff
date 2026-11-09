---
id: "02-aura-ash-exo/ash-desks"
title: "Ash desks"
kind: "collection"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile", "evidence:source"]
sources: [S-1f554a1713a5, S-356e0cb0a114, S-902271613c39, S-b28649c0301e, S-c3cf234a5550, S-f88b70fd0dfa, S-e4ff18df1f16]
---

# Ash desks

Ash desks supply membership, lifecycle observers, activation and movement. A global active-desk model does not supply independent per-output workspace semantics. [docs]

## What Chromium already has

The [row data](rows/aura-lift.tsv) holds 15 rows for this beat: ADAPT 5, FRESH 5, LIFT 5. These are historical source verdicts. [docs]

Donor availability in those rows: `chromeos-gated` 15. Zero Ash references in a file do not bypass its target build gate. [docs]

- Ash desks span displays and keep one active desk; per-output independent workspace selection has different state semantics. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.h#L29), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desks_controller.h#L541).
- Desk movement accepts a target root, separating window reassignment mechanics from the global desk activation model. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desks_controller.h#L254), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.h#L226).
- Ash uses one active desk container identifier on all display roots. Activation and deactivation update every associated container together, while per-root maps preserve container and stacking bookkeeping; the donor remains ChromeOS-gated. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desks_util.cc#L112), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.cc#L291).
- Ash desk migration moves the transient root and delegates descendant relocation. Cross-display moves first reparent into the destination desk on the original root, then move the hidden window to the target display. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.cc#L683), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.cc#L977).

## Domicile at the pin

- `manganese`: One page spans the desk; workspace switches are scoped to a screen, while floats keep a stable element across screens. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md#L142).
- The merged [workspace transition #1942](https://github.com/cprussin/domicile/pull/1942) separates slide motion from waiting for workspace pages. This is shell transition behavior, independent of the Ash desk controller. [docs]

## Boundary

Desk-container mutation, membership state and the displayed workspace are distinct. The canonical workspace comparison retains output scope, home and movement behavior. [inferred]

## Feature links

- [aura-wm/desks-and-virtual-desktops](../06-features/aura-wm/desks-and-virtual-desktops.md)

## Sources

- S-356e0cb0a114 · 2026-07-18 · Chromium ash desks source and equivalence inventory
- S-1f554a1713a5 · 2026-07-18 · Lift-map evaluation and corrections
- S-902271613c39 · 2026-11-09 · Pinned donor interface inspection
- S-b28649c0301e · 2026-11-09 · Pinned donor interface inspection
- S-c3cf234a5550 · 2026-11-09 · Pinned donor interface inspection
- S-f88b70fd0dfa · 2026-11-09 · Pinned donor interface inspection
- S-e4ff18df1f16 · 2026-11-09 pin check · Domicile desktop-page and presenter architecture
