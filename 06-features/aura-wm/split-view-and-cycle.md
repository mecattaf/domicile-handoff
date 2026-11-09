---
id: aura-wm/split-view-and-cycle
title: "Split view and window cycling"
kind: feature
family: aura-wm
layers: [manganese, sdk, compositor]
domicile_status: unknown
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
answered_upstream: false
scroll_parity: n/a
donor_availability: chromeos-gated
donors: ["ash"]
evidence_mix: mixed
boundary_note: false
bucket: domicile
tags: [family:aura-wm, donor:ash, evidence:source, bucket:domicile]
sources: [S-ea65f79d928a, S-82d30cf5a21b, S-7d51a3faaa84, S-17e38c60f482, S-537297eb1290, S-e77ffff40807]
eras: ["Aura lift (July 2026)", "views-shell"]
rows: ["AL-ash-splitview-cycle-001", "AL-ash-splitview-cycle-002", "AL-ash-splitview-cycle-003", "AL-ash-splitview-cycle-004", "AL-ash-splitview-cycle-005", "AL-ash-splitview-cycle-006", "AL-ash-splitview-cycle-007", "AL-ash-splitview-cycle-008", "AL-ash-splitview-cycle-009", "AL-ash-splitview-cycle-010", "AL-ash-splitview-cycle-011", "AL-ash-splitview-cycle-012", "AL-ash-splitview-cycle-013", "AL-ash-splitview-cycle-014", "AL-cade-niri-coverage-044", "AL-cade-niri-coverage-091", "AL-snap-tiling-scout-007", "AL-snap-tiling-scout-008", "AL-snap-tiling-scout-009", "AL-snap-tiling-scout-010", "AL-overview-desks-scout-018", "AL-engine-check-017"]
depends_on: []
reported_on_domicile: []
domicile_refs: ["docs/WRITING-A-SHELL.md", "docs/architecture/KEYBINDINGS.md"]
---

# Split view and window cycling

Cycling previews a candidate window and either commits that selection or restores the original activation. Split view separately constrains the bounds of participating windows. [source] ([Cycle contract](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_cycle/window_cycle_controller.h#L90))

## Behavior

- Preview navigation and committed activation remain distinct. [source] ([Cycle operations](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_cycle/window_cycle_controller.h#L72))
- Cycle availability changes while the screen is locked, and active-desk scoping is an explicit policy. [source] ([Availability](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_cycle/window_cycle_controller.h#L60), [Desk scope](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_cycle/window_cycle_controller.h#L127))

## How donors do it

| Donor @ pin | Mechanism | Notable behavior or limit | Link |
|---|---|---|---|
| Ash @ engine Chromium pin | `WindowCycleController` installs a pre-target handler for modifier release | Completion activates the candidate; cancellation preserves the original window | [Complete and cancel](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_cycle/window_cycle_controller.h#L90) |


- ChromeOS-gated Ash cycling normally uses a nonactivatable widget and a root-scoped custom event targeter. Spoken feedback instead permits widget activation and skips that targeter. Initialization also starts throttling the cycle candidates. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_cycle/window_cycle_list.cc#L378).

## What the notes found

- The 2026-10-01 census separated cycle views from its MRU list, event filter and activation controller. Captured thumbnails and a live layer mirror have different data sources. [docs] S-82d30cf5a21b.
- The census kept split-view layout, snap groups and window cycling as distinct mechanisms, despite their shared window-state vocabulary. [docs] S-7d51a3faaa84.

## Where it touches Domicile

- The [shell authoring contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md) and [keybinding model](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/KEYBINDINGS.md) provide the Domicile comparison points at the pin.
- A shell can present a candidate list while the SDK and compositor retain activation and native state delivery; Ash’s pre-target event ownership does not transfer with the list view. [inferred]

## Lineage

- 2026-07-18: the Aura study cataloged split-view and cycle behavior. [docs]
- 2026-10-01: the client-shell census isolated preview presentation from compositor authority. [docs]

## Sources

- S-ea65f79d928a · 2026-11-09 · Chromium window cycle interface
- S-82d30cf5a21b · 2026-10-01 · Ash and client-shell window authority study
- S-7d51a3faaa84 · 2026-10-01 · Ash component and controller census
- S-17e38c60f482 · 2026-11-09 · Ash state, display-root or preview implementation
- S-537297eb1290 · 2026-11-09 pin check · Pinned shell keyboard binding contract
- S-e77ffff40807 · 2026-11-09 pin check · Pinned shell authoring contract
