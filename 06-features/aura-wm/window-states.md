---
id: aura-wm/window-states
title: Window state transitions
kind: feature
family: aura-wm
layers: [manganese, sdk, compositor]
domicile_status: unknown
checked_at: "2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc"
answered_upstream: false
scroll_parity: n/a
donor_availability: chromeos-gated
donors: [ash, exo]
evidence_mix: mixed
boundary_note: false
bucket: domicile
tags: [family:aura-wm, donor:ash, evidence:source, bucket:domicile]
sources: [S-2f2973c4cda8, S-327f70253fe5, S-3533e3f0274a, S-4111d1fe471f, S-578852a49413, S-5a1d66b34064, S-6da8864cbb54, S-82d30cf5a21b, S-a59107f9d164, S-b90f393c5499, S-be5932a4f7a2, S-c153d333f325, S-e2a96179b3b7, S-e77ffff40807]
eras: [Aura lift (July 2026), views-shell]
rows: [AL-ash-wm-states-001, AL-ash-wm-states-002, AL-ash-wm-states-004, AL-ash-wm-states-005, AL-ash-wm-states-006, AL-ash-wm-states-007, AL-ash-wm-states-008, AL-ash-wm-states-009, AL-ash-wm-states-010, AL-ash-wm-states-011, AL-ash-wm-states-012, AL-ash-wm-states-013, AL-ash-wm-states-014, AL-ash-wm-states-015, AL-ash-wm-states-017, AL-ash-wm-states-018, AL-ash-wm-states-019, AL-ash-wm-states-020, AL-ash-wm-states-021, AL-ash-wm-states-022, AL-ash-wm-states-023, AL-ash-wm-states-024, AL-ash-wm-states-027, AL-ash-wm-states-028, AL-aura-wm-core-028, AL-exo-core-009, AL-exo-core-011, AL-exo-core-031, AL-exo-wayland-033, AL-cubs-coverage-011, AL-cubs-coverage-012, AL-cade-machinery-wm-004, AL-cade-machinery-wm-005, AL-cade-machinery-wm-007, AL-cade-machinery-wm-020, AL-cade-machinery-wm-021, AL-cade-spec-coverage-022, AL-cade-niri-coverage-034, AL-cade-niri-coverage-035, AL-cade-niri-coverage-090, AL-cade-audit-tables-001, AL-machinery-central-015, AL-april-protocol-triage-007, AL-april-protocol-triage-022, AL-april-reverse-triage-020, AL-tree-scout-002, AL-tree-scout-006, AL-tree-scout-035, AL-june-exo-coverage-008, AL-june-machinery-review-036, AL-june-machinery-review-037, AL-june-machinery-review-040, AL-june-machinery-review-041, AL-rt-tree-findings-010, AL-rt-tree-findings-011, AL-rt-tree-findings-019, AL-snap-tiling-scout-001, AL-snap-tiling-scout-002, AL-snap-tiling-scout-003, AL-snap-tiling-scout-004, AL-snap-tiling-scout-005, AL-snap-tiling-scout-006, AL-aura-framework-003, AL-aura-framework-004, AL-wm-column-deliberation-004, AL-wm-column-deliberation-005, AL-wm-column-deliberation-006, AL-wm-column-deliberation-007, AL-wm-column-deliberation-008, AL-wm-column-deliberation-009, AL-wm-column-deliberation-010, AL-wm-column-deliberation-011, AL-wm-column-deliberation-012, AL-wm-column-deliberation-013, AL-wm-column-deliberation-014, AL-engine-check-005]
depends_on: []
reported_on_domicile: []
domicile_refs: [docs/WRITING-A-SHELL.md]
---
# Window state transitions

Restoring a window can require both earlier geometry and a compatible prior state. State requests and the eventual client response can be separate events. [source] ([Ash state contract](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/window_state.h#L489))

## Behavior

- Normal, maximized, minimized, fullscreen and snapped state are distinct from current bounds in the donor. [source] ([State model](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/window_state.h#L67))
- The donor stores restore geometry and a stack of prior states; incompatible history can be pruned. [source] ([Restore history](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/window_state.h#L555))

## How donors do it

| Donor @ pin | Mechanism | Notable behavior or limit | Link |
|---|---|---|---|
| Ash @ Chromium 150 | A per-window `State` handles `WMEvent` transitions | Client-controlled requests can be declined or applied later; direct bounds bypass the request path | [State contract](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/window_state.h#L80) |

- At the engine pin, ordinary [Aura bounds requests](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window.cc#L617) can delegate to the parent layout manager. Ash state transitions also use a [direct helper](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/layout_manager.cc#L17) through [WindowState](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L197); [maximize/fullscreen handling](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/default_state.cc#L461) takes that path. A parent layout hook therefore does not intercept every bounds change. [source]


- ChromeOS-gated Exo applies pending geometry, display, shape and size constraints on commit; initial showing waits for nonempty host bounds, with activation separately checked by its security delegate. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L2358), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L2403), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L2477). [Exo target gate](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/BUILD.gn#L13).


- ChromeOS-gated Ash restore history uses ordered state layers, prunes incompatible entries and retains snap-group membership. Restoration also considers tablet mode and existing floated windows. These are ChromeOS window policies. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L112), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L852), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L1374).

- ChromeOS-gated Ash converts show-state property changes into window-manager events, but reverts external writes to its internal state-type property. Replacing the state object detaches the old policy and attaches its replacement. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L747), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L1469).

- ChromeOS-gated Ash restoration may select another display from restored screen bounds when they miss the current display. No explicit restore-display identity is stored here; fullscreen can also re-enter its state to target another display. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/default_state.cc#L76), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/default_state.cc#L118).

- ChromeOS-gated Exo configure acknowledgment accumulates queued origin offsets and retains the matching configuration until commit; pre-commit then applies the acknowledged resize direction and LocalSurfaceId before releasing that retained configuration. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface.cc#L208), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface.cc#L891).

## What the notes found

- A 2026-10-01 study separated the state model and observers from the authority that mutates the window tree. A client shell retained the former concepts while requesting changes from its compositor. [docs] S-82d30cf5a21b.
- 2026-11-06: Domicile exposed XDG fullscreen and tiled states; the native Resizing state was not exposed as of that snapshot, while visual resize synchronization was present. [docs]

## Where it touches Domicile

- The [shell authoring contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md) describes the Domicile model at the pin. Ash state names do not establish identical Domicile behavior. [inferred]
- `State::OnWMEvent` and the SDK shell model are candidate locations for comparing transition invariants; native client state remains part of compositor delivery. [inferred]

- At the pinned checkout, the SDK exposes tiled-edge, maximized and fullscreen flags. Client requests are reported to the shell; its decision travels back through `setStates`. [source] [State contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/window-state.ts#L5), [native element state delivery](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/AppWindow.tsx#L226).

- Native submissions choose a stored configure box by sequence, preserve its device scale and retain an older acknowledged size rather than stretching its buffer over a newer box. [source] [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L271), [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L327), [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L362).

## Lineage

- 2026-07-18: the Aura study cataloged Ash state transitions. [docs]
- 2026-10-01: the client-shell study replaced direct Aura mutations with requests and observed state. [docs]

## Sources

- S-327f70253fe5 · 2026-11-09 · Chromium window state interface
- S-82d30cf5a21b · 2026-10-01 · Ash and client-shell window authority study
- S-a59107f9d164 · 2026-11-09 · Pinned native-window state and request enumeration
- S-e2a96179b3b7 · 2026-11-09 · Pinned native-window state delivery
- S-578852a49413 · 2026-11-09 · Aura ordinary bounds dispatch
- S-b90f393c5499 · 2026-11-09 · Aura direct child-bounds helper
- S-6da8864cbb54 · 2026-11-09 · Ash state bounds and animation paths
- S-5a1d66b34064 · 2026-11-09 · Ash maximize, fullscreen and snap bounds application
- S-c153d333f325 · 2026-11-09 · Domicile native import, frame construction and feedback routing
- S-be5932a4f7a2 · 2026-11-09 · Exo shell-surface shape, commit and popup lifecycle
- S-4111d1fe471f · 2026-11-09 · Exo build-target gate
- S-e77ffff40807 · 2026-11-09 pin check · Pinned shell authoring contract
- S-2f2973c4cda8 · 2026-11-09 · Exo configure, commit-target, transition and transient-parent paths
