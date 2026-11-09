---
id: effects/corner-radius-shadows
title: Window clipping, shadows and dimming
kind: feature
family: effects
layers: [engine:blink, engine:viz, manganese]
domicile_status: unknown
checked_at: "2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc"
answered_upstream: false
scroll_parity: beyond
donor_availability: in-engine-build
donors: [aura, ash, swayfx, exo]
evidence_mix: mixed
boundary_note: false
bucket: domicile
tags: [family:effects, donor:aura, evidence:source, bucket:domicile, scroll-parity:beyond]
sources: [S-0c4d3fd4676a, S-136eae3a0b23, S-204a969601d0, S-292e57b07af7, S-4111d1fe471f, S-4c2392ac6a77, S-50c123e64a11, S-578852a49413, S-5b1bd336dbff, S-697de35f64e0, S-6a02b449627e, S-6da8864cbb54, S-703c0d408b29, S-78a98efb6748, S-83c6a5bbb592, S-900dfbb65723, S-acf45a44d844, S-be5932a4f7a2, S-c153d333f325, S-c3368d7ab705, S-de6569bcc761, S-e0dd6eebeebc, S-e113eec0f613, S-e8c9a1fa0f87]
eras: [Aura lift (July 2026), views-shell]
rows: [AL-ash-sysui-blur-wallpaper-003, AL-aura-wm-core-009, AL-aura-wm-core-013, AL-aura-wm-core-025, AL-aura-wm-core-026, AL-exo-wayland-036, AL-cade-triage-038, AL-cade-triage-105, AL-cade-triage-106, AL-cade-triage-108, AL-cade-decision-025, AL-cade-decision-092, AL-cade-decision-093, AL-cade-decision-095, AL-cade-machinery-wm-012, AL-cade-machinery-wm-014, AL-cade-machinery-wm-018, AL-cade-machinery-wm-019, AL-cade-minimal-ash-017, AL-cade-niri-coverage-042, AL-cade-niri-coverage-063, AL-cade-niri-coverage-074, AL-cade-niri-coverage-076, AL-april-protocol-triage-019, AL-april-protocol-triage-039, AL-april-reverse-triage-012, AL-april-reverse-triage-013, AL-april-reverse-triage-026, AL-april-reverse-triage-034, AL-april-reverse-triage-035, AL-april-reverse-triage-039, AL-april-reverse-triage-055, AL-april-reverse-triage-059, AL-april-reverse-triage-064, AL-april-reverse-triage-065, AL-april-reverse-triage-067, AL-june-exo-coverage-019, AL-aura-vs-mir-003, AL-aura-vs-mir-030, AL-engine-check-020]
depends_on: []
reported_on_domicile: []
domicile_refs: [docs/architecture/WINDOW-COMPOSITING.md]
---
# Window clipping, shadows and dimming

Clipping changes the visible window content. A shadow paints outside that content, and dimming changes its presentation without changing the underlying client state. [source] ([Shadow controller](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ui/wm/core/shadow_controller.h#L32))

## Behavior

- Layer clipping, rounded-corner clipping and event acceptance have separate controls. Accepting events on a layer does not change its descendants. [source] ([Layer contract](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.h#L393))
- Painted shadow bounds and input bounds remain separate. [docs] S-0c4d3fd4676a.
- Non-fast rounded corners with multiple drawing descendants can cause a render surface. [source] ([Render-surface reason](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/cc/trees/property_tree_builder.cc#L369))
- Shadow updates follow activation and window property changes in the Aura donor. [source] ([Activation observer](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ui/wm/core/shadow_controller.h#L56))

## How donors do it

| Donor @ pin | Mechanism | Notable behavior or limit | Link |
|---|---|---|---|
| Aura @ Chromium 150 | `ShadowController` observes windows and activation | Shadow visibility and elevation colors are separate from the window’s client buffer | [Shadow lifecycle](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ui/wm/core/shadow_controller.h#L32) |
| Ash survey · 2026-10-01 | View-based and window-observing shadows have different ownership | A separate client paints inside an expanded transparent surface and excludes shadow pixels from its input region | S-0c4d3fd4676a |
| Swayfx @ 0.6 | Compositor rules separate shadow, corner and dim controls | Per-window shadow enablement differs from global radius, client decorations and inactive or urgent dim state | [Effect controls](https://github.com/wlrfx/swayfx/blob/0.6/sway/sway.5.scd#L705) |

- Default Aura targeting intersects mouse and touch bounds with the layer clip rectangle. Extra shape rectangles use a separate virtual hook; this targeter does not consult painted rounded corners or pixel alpha. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window_targeter.cc#L37), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window_targeter.cc#L358).

- Aura’s [default occluder test](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window_occlusion_tracker.cc#L55) requires drawn, nontransparent content, combined opacity one and no ancestor alpha shape. [Declared opaque regions](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window.h#L612) instead describe coverage of windows below; they change neither the declaring window’s pixels nor its own coverage test. [Changes notify the tracker](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window.cc#L1851). These are occlusion declarations, distinct from input targeting. [source]


- ChromeOS-gated Exo commits an explicit window shape to its layer and hit-test mask. Setting that shape requires a frameless window, disables server resize handles and removes shadows. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L635), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L1439), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L2578). [Exo target gate](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/BUILD.gn#L13).


- When ChromeOS-gated Ash manages rounded-window opacity, it marks the window transparent and explicitly declares rectangular bounds opaque for lower-window occlusion. Bounds changes refresh that declaration; this metadata does not define rounded input regions. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L323), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L1026), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L1560).

- ChromeOS-gated Ash mirror views can remove a shadow subtree using a decoration-container name or matching negative-offset shadow bounds; this is an explicit preview option with heuristic identification. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_mirror_view.cc#L67), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_mirror_view.cc#L240).

- Generic Aura shadows are window-owned decorations with a child layer stacked at the bottom of the window layer’s children. Creation starts window occlusion tracking; occluded windows suppress shadow eligibility. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/shadow_controller.cc#L349), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/shadow_controller.cc#L280).

- Without a delegate, generic shadow eligibility rejects fullscreen and maximized windows and requires positive elevation. A delegate replaces that state-based decision, while occlusion still suppresses the shadow. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/shadow_controller.cc#L280).

- Generic shadow rounding copies the upper-left radius into a uniform corner radius. A delegate may disable shadow rounding; this does not set the window’s painted corners or input shape. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/shadow_controller.cc#L311).

- Generic shadow activation retains active elevation when activation moves to a transient child that hides on deactivation. Explicit elevation properties and default-elevation checks separately constrain activation updates. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/shadow_controller.cc#L41), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/shadow_controller.cc#L258).

- Generic Layer opacity updates compositor opacity and schedules drawing. Visibility instead hides the layer subtree and updates that layer’s hit-test flag from visibility and event acceptance; descendants have their own event-acceptance flags. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.cc#L1418), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.h#L666), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.h#L710).

## What the notes found

- The October inventory distinguished SystemViewShadow from the variant observing an Aura window. That is an ownership difference, independent of the material palette. [docs] S-0c4d3fd4676a.
- 2023-04-29: Any1 container-local client state inherits from its parent then global values; global bypasses current container context, default clears the local override, and updates rebuild descendant state (any1@9d60831231375110a7d1b03d52a5c7df674b40bc) [source] <https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/commands/client.c#L82> <https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/commands/client.c#L129> <https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/tree/container.c#L1981>
- 2023-04-29: Any1 rebuilds a locally updated container and its descendants only while the configuration is active; the default reset still clears its local override (any1@9d60831231375110a7d1b03d52a5c7df674b40bc) [source] <https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/commands/client.c#L82>
- 2026-11-09: Shadow exclusion from hit tests and input mapping through the displayed animated scale remain unverified at the Domicile pin for Scroll contracts SC-0522 and SC-0523 (scroll@b6586d273fed7d187744b7eaec2c234386a029fa) [inferred] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/scene.c#L79> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L50> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1762>

## Where it touches Domicile

- The [window-compositing contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md) provides the current page-element and native-surface boundary.
- Native app content enters the page’s property trees, so ordinary CSS clip, opacity, filter and blend apply to its foreign layer. [source] ([Paint boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/layout_app_surface.cc#L57))

- Native frame construction samples crop-derived normalized texture coordinates to exclude client-drawn shadows. CropToUv returns the complete texture when either crop or buffer size is empty. [source] [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/surface_crop.h#L13), [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/surface_crop.cc#L8), [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L314).

## Lineage

- 2026-07-18: the Aura study cataloged layer clipping and shadow machinery. [docs]
- 2026-10-01: the client-shell census isolated shadow painting from input-region and lifetime responsibilities. [docs]

## Sources

- S-136eae3a0b23 · 2026-11-09 · Chromium activation-aware shadow interface
- S-de6569bcc761 · 2026-11-09 · Chromium layer effect and clipping interface
- S-6a02b449627e · 2026-11-09 · Chromium compositor render-surface reason calculation
- S-5b1bd336dbff · 2026-11-09 · Domicile native app layout and foreign-layer paint implementation
- S-0c4d3fd4676a · 2026-10-01 · Ash surface portability inventory
- S-e0dd6eebeebc · 2026-10-22 · Swayfx visual-effect control reference
- S-292e57b07af7 · 2026-10-22 · Compositor effect and lifetime study
- S-900dfbb65723 · 2026-11-09 · Aura default geometry, policy and subtree targeting
- S-e113eec0f613 · 2026-11-09 · Domicile native crop contract
- S-4c2392ac6a77 · 2026-11-09 · Domicile native crop texture coordinates
- S-c153d333f325 · 2026-11-09 · Domicile native import, frame construction and feedback routing
- S-697de35f64e0 · 2026-11-09 · Aura explicit opaque-region contract
- S-578852a49413 · 2026-11-09 · Aura opaque-region observer notification
- S-acf45a44d844 · 2026-11-09 · Aura default and explicit occlusion coverage paths
- S-be5932a4f7a2 · 2026-11-09 · Exo shell-surface shape, commit and popup lifecycle
- S-4111d1fe471f · 2026-11-09 · Exo build-target gate
- S-6da8864cbb54 · 2026-11-09 · Ash state, display-root or preview implementation
- S-c3368d7ab705 · 2026-11-09 · Ash state, display-root or preview implementation
- S-703c0d408b29 · 2026-11-09 pin check · Pinned native-window scene embedding and client anchors
- S-e8c9a1fa0f87 · 2026-11-09 · Aura shadow ownership, eligibility and activation policy
- S-83c6a5bbb592 · 2026-11-09 · Chromium layer property, content ownership and lifetime contract
