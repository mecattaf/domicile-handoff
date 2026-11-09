---
id: aura-wm/overview-mode
title: "Overview over live windows"
kind: feature
family: aura-wm
layers: [engine:blink, sdk, manganese]
domicile_status: landed
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
answered_upstream: false
scroll_parity: n/a
donor_availability: chromeos-gated
donors: ["ash"]
evidence_mix: mixed
boundary_note: false
bucket: domicile
tags: [family:aura-wm, donor:ash, evidence:source, bucket:domicile]
sources: [S-4a6520490f9a, S-82d30cf5a21b, S-e19473fe102a, S-4ea478cdaf86, S-8d9c52f81df5, S-c3368d7ab705, S-703c0d408b29]
eras: ["Aura lift (July 2026)", "views-shell"]
rows: ["AL-ash-overview-001", "AL-ash-overview-002", "AL-ash-overview-003", "AL-ash-overview-004", "AL-ash-overview-005", "AL-ash-overview-006", "AL-ash-overview-007", "AL-ash-overview-008", "AL-ash-overview-009", "AL-ash-overview-010", "AL-ash-overview-011", "AL-ash-overview-012", "AL-ash-overview-013", "AL-ash-overview-014", "AL-ash-overview-016", "AL-ash-overview-017", "AL-ash-overview-018", "AL-ash-overview-019", "AL-ash-overview-020", "AL-ash-overview-021", "AL-ash-overview-022", "AL-cade-audit-tables-002", "AL-april-reverse-triage-010", "AL-tree-scout-005", "AL-rt-tree-findings-014", "AL-engine-check-008"]
depends_on: []
reported_on_domicile: []
domicile_refs: ["cprussin/domicile#1931", "cprussin/domicile#1940", "docs/architecture/WINDOW-COMPOSITING.md"]
---

# Overview over live windows

Overview changes the presentation of existing windows while preserving the state required to leave overview. The interaction also changes input targeting, transient handling and clipping. [source] ([Overview helper](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.h#L37))

## Behavior

- Transient descendants participate in the donor overview item. [source] ([Transient handling](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.h#L141))
- The helper [records target opacity and target clipping](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L144). [Restoration](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L242) distinguishes direct resets, optional animated transform reset and delayed clip clearing. [source]

## How donors do it

| Donor @ pin | Mechanism | Notable behavior or limit | Link |
|---|---|---|---|
| Ash @ engine pin · chromeos-gated | `ScopedOverviewTransformWindow` manages a real window and transient descendants | Explicit restore branches and scoped input blocking; minimized or tucked items use a separate overview host | [Overview contract](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.h#L37) |

- [Per-transient input scopes](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L118) block targeting and temporarily suppress bubble offscreen adjustment. [Live child additions and removals](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L589) acquire and release those scopes. [source]
- [Tree restoration](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L707) restores an unfinished drag’s synchronizer before the main one. This closes a separate lifetime from animation completion. [source]


- ChromeOS-gated Ash window previews exclude popup-type transients, defer unparented transient children until parenting completes and remove all mirror views when their main source window is destroyed. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_preview_view.cc#L117), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_preview_view.cc#L143), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_preview_view.cc#L165).

- ChromeOS-gated Ash mirror views create a force-visible occlusion scope for their source; minimized, tucked or inactive-desk sources also cause mirrored window layers to be made visible with full opacity. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_mirror_view.cc#L192), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_mirror_view.cc#L222).

## What the notes found

- A 2026-10-01 study distinguished transformations of real windows from a client-side grid of captured images. The latter does not gain authority over foreign windows through its visuals. [docs] S-82d30cf5a21b.

## Where it touches Domicile

- The [window-compositing contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md) explains native windows represented by page elements at the pin.
- Ash’s temporary transform and Manganese overview under CSS transforms are a candidate semantic equivalence. The equivalent invariants are restoration, transient membership and input targeting; implementation identity is not established. [inferred]

- Status at the pinned checkout: `landed`. The overview uses CSS transforms with separate monitor clipping and a transition that can reverse from its current position. [source] [Current transform and clip](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/overview/seen-styles.ts#L8); [cprussin/domicile#1931](https://github.com/cprussin/domicile/pull/1931), [#1940](https://github.com/cprussin/domicile/pull/1940).

## Lineage

- 2026-07-18: the Aura study cataloged overview transforms and window mirrors. [docs]
- 2026-10-01: the client-shell study isolated live-window transformation from thumbnail presentation. [docs]

## Sources

- S-4a6520490f9a · 2026-11-09 · Chromium overview restoration interface
- S-82d30cf5a21b · 2026-10-01 · Ash and client-shell window authority study
- S-e19473fe102a · 2026-11-09 · Pinned overview transform and clipping
- S-4ea478cdaf86 · 2026-11-09 · Ash overview target-state restoration, transient input scopes and drag cleanup
- S-8d9c52f81df5 · 2026-11-09 · Ash state, display-root or preview implementation
- S-c3368d7ab705 · 2026-11-09 · Ash state, display-root or preview implementation
- S-703c0d408b29 · 2026-11-09 pin check · Pinned native-window scene embedding and client anchors
