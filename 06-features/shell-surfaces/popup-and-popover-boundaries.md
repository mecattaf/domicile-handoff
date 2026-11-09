---
id: shell-surfaces/popup-and-popover-boundaries
title: Popup membership and dismissal
kind: feature
family: shell-surfaces
layers: [engine:aura, engine:blink, sdk, manganese]
domicile_status: unknown
checked_at: "2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc"
answered_upstream: false
scroll_parity: n/a
donor_availability: in-engine-build
donors: [aura, exo]
evidence_mix: mixed
boundary_note: false
bucket: domicile
tags: [family:shell-surfaces, donor:aura, evidence:source, bucket:domicile]
sources: [S-0ad85bed39c5, S-0c4d3fd4676a, S-1311eba62ca0, S-2f2973c4cda8, S-3533e3f0274a, S-3abebbad32a3, S-4111d1fe471f, S-540f3290d528, S-578852a49413, S-58480e08aa90, S-6f32cfa621b2, S-7db6c4abc1a7, S-81826624bc96, S-900dfbb65723, S-9840f843a070, S-a514b4f6d553, S-a89a4c46b8f9, S-b056064c5b44, S-bc2a6582ae19, S-be5932a4f7a2, S-cf224d65065a]
eras: [Aura lift (July 2026), views-shell]
rows: [AL-aura-wm-core-007, AL-exo-core-008, AL-exo-core-010, AL-exo-core-036, AL-views-inventory-030, AL-views-inventory-083, AL-views-inventory-153, AL-views-inventory-154, AL-cade-triage-190, AL-cade-decision-177, AL-cubs-coverage-016, AL-cubs-coverage-039, AL-cubs-coverage-043, AL-cubs-coverage-044, AL-cade-audit-tables-010, AL-april-protocol-triage-002, AL-april-protocol-triage-008, AL-april-protocol-triage-041, AL-april-reverse-triage-004, AL-april-reverse-triage-014, AL-april-reverse-triage-030, AL-june-exo-coverage-009, AL-ash-shell-inventory-003, AL-ash-shell-inventory-005, AL-ash-shell-inventory-009, AL-compositor-correctness-002, AL-compositor-correctness-010, AL-compositor-correctness-013, AL-compositor-correctness-021, AL-dmabuf-scout-002, AL-dmabuf-scout-007, AL-dmabuf-design-014, AL-dmabuf-design-019, AL-surface-matrix-033, AL-surface-matrix-034, AL-engine-check-014]
depends_on: []
reported_on_domicile: []
domicile_refs: [docs/architecture/STACKING-PARITY.md]
---
# Popup membership and dismissal

A popup belongs to a parent interaction even when it occupies a separate surface. Parenting affects lifetime, visibility, stacking and which input reaches the underlying window. [source] ([Transient membership](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/transient_window_manager.h#L20))

## Behavior

- Popup membership, paint stacking and input modality remain distinct. [source] ([Modality contract](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/window_modality_controller.h#L38))
- Hiding and showing a parent do not imply identical child visibility behavior; re-show propagation is configurable and off by default in the donor. [source] ([Visibility policy](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/transient_window_manager.h#L56))

## How donors do it

| Donor @ pin | Mechanism | Notable behavior or limit | Link |
|---|---|---|---|
| Aura @ engine pin | `TransientWindowManager` tracks parent and child relationships | Parent hiding propagates; destruction and automatic re-show each have explicit controls | [Transient lifetime](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/transient_window_manager.h#L20) |
| Aura @ engine pin | `WindowModalityController` filters events | Modal children consume input directed to their transient parents | [Modality contract](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/window_modality_controller.h#L38) |
| Exo @ engine pin · chromeos-gated | Located-event routing walks transient ancestors with coordinate conversion | Non-client resize and caption-control regions stop that fallback | [Routing](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_util.cc#L193), [target gate](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/BUILD.gn#L13) |

- Showing an Aura modal transient cancels active touches across its transient tree and releases conflicting capture; child-modal capture outside the modal parent’s subtree is exempt. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/window_modality_controller.cc#L179), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/window_modality_controller.cc#L240).
- Aura removes a dying window from its transient parent before destroying dependent children; children configured for independent lifetime are detached instead of deleted. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/transient_window_manager.cc#L292).
- Aura checks subtree visibility, event-targeting policy and EventClient permission before geometric descent. Reverse child traversal converts located-event coordinates into each candidate before recursive targeting. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window_targeter.cc#L260), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window_targeter.cc#L329), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window.cc#L2005), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/events/event_target_iterator.h#L22).


- ChromeOS-gated Exo popup capture loss preserves the new capture window and its transient ancestors, then closes other grabbed branches and descendants from leaves toward roots; screenshot policy can suppress this dismissal. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L1469), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L1483), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L1537), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L1569).

- ChromeOS-gated Exo surface destruction detaches shell-surface transient children before hiding the widget and invokes its destruction callback last; ordinary close requests defer immediate widget destruction. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L1364), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L1390). [Exo target gate](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/BUILD.gn#L13).

- ChromeOS-gated Exo rejects transient-parent cycles and enables parent-controlled visibility only for activatable top-level children; changing parents clears previous visibility control before unlinking. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface.cc#L967), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface.cc#L991), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface.cc#L1007).

## What the notes found

- A 2026-10-01 inventory identified outside-click dismissal as a global pre-target filter in Ash. A separate Wayland client does not receive arbitrary clicks on another application; popup grabs and focus changes form a different boundary. [docs] S-0c4d3fd4676a.
- 2026-11-09: At the pin a source comment records popups as members of every display at the densest scale, because the page reports no popup bounds and popups do not follow the parent's placement (#933) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/outputs.rs#L409> <https://github.com/cprussin/domicile/pull/933>
- 2026-11-05: sway's popup_during_fullscreen has three policies: smart (default) shows the dialog, ignore drops it, leave_fullscreen exits fullscreen and shows it (sway@1652c54) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L857-L861>
- 2026-10-23: Native layer-surface options expose anchoring, exclusive-zone and keyboard interactivity, but compositor-selected placement and per-window lifetime control are separate contracts; initial show:false and activation are not evidence of a per-window hide API (GPUIX@a53b294401280014e94eb9bd98a7c7ed1929f92d) [docs] <https://github.com/remorses/gpuix/blob/a53b294401280014e94eb9bd98a7c7ed1929f92d/packages/native/src/renderer.rs>
- 2026-10-06: An Otto panel can regain keyboard focus after an overlay closes; an on-demand panel releases keyboard focus on an outside click. These are explicit role and focus-lifetime contracts (otto@v1.6.0) [docs]
- 2026-11-06: Manganese popups rendered as siblings with parent x/y offsets and did not follow the window's scale, rotation or motion as of 2026-11-06 [docs]

## Where it touches Domicile

- The pinned [stacking contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/STACKING-PARITY.md) distinguishes Domicile’s native-window and page-element interactions.
- The SDK makes [focus requests](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/app-element.ts#L18) cancelable, and separately exposes a [focus-release request](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/app-element.ts#L39) when a press misses every native window. The shell can retain focus for its own window controls using the pressed element. This is a focus-policy hook; it does not alone specify popup-grab or dismissal behavior. [source]
- The equivalent invariants are lifetime, parent visibility, activation and allowed input. A visual popover boundary alone does not prove those invariants. [inferred]

## Lineage

- 2026-07-18: the Aura study cataloged transient parenting and modality. [docs]
- 2026-10-01: the client-shell inventory isolated dismissal, anchoring and grabs as separate runtime seams. [docs]

## Sources

- S-81826624bc96 · 2026-11-09 · Chromium transient membership interface
- S-540f3290d528 · 2026-11-09 · Chromium modal input interface
- S-0c4d3fd4676a · 2026-10-01 · Ash surface portability inventory
- S-bc2a6582ae19 · 2026-11-09 · Domicile native-element focus and presentation event contract
- S-1311eba62ca0 · 2026-11-09 · Aura modal touch cancellation and capture rules
- S-6f32cfa621b2 · 2026-11-09 · Aura transient destruction and independent lifetime
- S-900dfbb65723 · 2026-11-09 · Aura default geometry, policy and subtree targeting
- S-578852a49413 · 2026-11-09 · Aura window visibility and event completion
- S-0ad85bed39c5 · 2026-11-09 · Chromium reverse event-target iteration
- S-58480e08aa90 · 2026-11-09 · Exo located-event routing and transient coordinate conversion
- S-4111d1fe471f · 2026-11-09 · Exo build-target gate
- S-be5932a4f7a2 · 2026-11-09 · Exo shell-surface shape, commit and popup lifecycle
- S-3abebbad32a3 · 2026-11-09 pin check · Pinned stacking and transient-window contract
- S-2f2973c4cda8 · 2026-11-09 · Exo configure, commit-target, transition and transient-parent paths
