---
id: effects/window-animations
title: Window animation lifetime and scene transitions
kind: feature
family: effects
layers: [engine:blink, engine:viz, manganese]
domicile_status: landed
domicile_refs: [cprussin/domicile#1910, cprussin/domicile#1939, cprussin/domicile#1940]
checked_at: "2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc"
answered_upstream: false
scroll_parity: n/a
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:effects, status:landed, bucket:domicile, evidence:source]
sources: [S-14b41dc319ce, S-1bcd456a94e6, S-292e57b07af7, S-2f2973c4cda8, S-3533e3f0274a, S-4111d1fe471f, S-47f30e0b6caa, S-4a2b900f6a8d, S-4a6520490f9a, S-4a9f3490b0b6, S-502e32dd38c7, S-55c701699898, S-578852a49413, S-581a3e7127e2, S-6b5cc0eed377, S-6bec7a90d975, S-6da8864cbb54, S-70ac6562eb03, S-7cb0007051a1, S-7e62b17cb4f1, S-8191329f44ce, S-83c6a5bbb592, S-85179425f20e, S-963f6f0016cd, S-9e14c851272d, S-a6ccc227ff5c, S-acf45a44d844, S-af414fdb64d0, S-bac395effe1f, S-be5932a4f7a2, S-c3368d7ab705, S-cd1972571051, S-cfd50c0feaf3, S-de6569bcc761, S-de92af3f5a9b, S-e19473fe102a, S-e2a96179b3b7, S-e34b121eace4]
donor_availability: in-engine-build
donors: [aura, ash, kde, niri, swayfx, exo]
eras: [Aura lift (July 2026)]
rows: [AL-aura-wm-core-008, AL-aura-wm-core-027, AL-compositor-blur-anim-010, AL-compositor-blur-anim-011, AL-compositor-blur-anim-012, AL-compositor-blur-anim-013, AL-compositor-blur-anim-014, AL-compositor-blur-anim-015, AL-compositor-blur-anim-016, AL-compositor-blur-anim-017, AL-compositor-blur-anim-018, AL-compositor-blur-anim-019, AL-compositor-blur-anim-020, AL-compositor-blur-anim-021, AL-compositor-blur-anim-022, AL-views-inventory-048, AL-cade-triage-037, AL-cade-triage-109, AL-cade-triage-110, AL-cade-triage-111, AL-cade-triage-202, AL-cade-triage-213, AL-cade-decision-024, AL-cade-decision-096, AL-cade-decision-097, AL-cade-decision-098, AL-cade-decision-189, AL-cade-decision-200, AL-cade-machinery-wm-002, AL-cade-machinery-wm-003, AL-cade-spec-coverage-006, AL-april-protocol-triage-038, AL-april-reverse-triage-002, AL-april-reverse-triage-066, AL-tree-scout-003, AL-tree-scout-009, AL-june-machinery-review-053, AL-rt-tree-findings-012, AL-rt-tree-findings-018, AL-wm-column-policy-024, AL-aura-vs-mir-004, AL-aura-vs-mir-031, AL-aura-vs-mir-057, AL-ash-layout-mechanism-inputs-003, AL-engine-check-012]
depends_on: []
reported_on_domicile: []
---
# Window animation lifetime and scene transitions

Windows can move, fade, change workspace and leave the scene while their content remains available for the transition. The shell keeps animation state separate from the live window list. [source] [Motion state](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindowMotion.ts#L171)

## Behavior

- A closed or departing window remains drawn until its animation ends.
- A transition interrupted halfway resumes from its current position.
- A float crossing monitors keeps the same element and browser guest.
- A departing native element is inert and stops requesting keyboard focus. [source] [Native window lifetime](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/AppWindow.tsx#L105)

## How donors do it

| Donor @ pin | Mechanism | Boundary | Link |
|---|---|---|---|
| Aura @ Chromium 150 | LayerAnimator and LayerAnimationElement | Layer properties and animation ownership | [Animator](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ui/compositor/layer_animator.h) |
| Ash @ Chromium 150 | Scoped overview transform | Live window and transient restoration | [Overview scope](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/overview/scoped_overview_transform_window.h) |
| KWin @ linked pin | Closed-window state is retained through an effect reference guard; a spring can change its target while retaining motion state | Resource lifetime and spring retargeting are distinct from selecting an animation curve | [Effect lifetime](https://github.com/KDE/kwin/blob/0406762df7794213f12ce87bc8f6fd322dcdc53c/src/effect/effectwindow.h#L841), [spring state](https://github.com/KDE/kwin/blob/0406762df7794213f12ce87bc8f6fd322dcdc53c/src/effect/springmotion.cpp#L118) |
| Niri @ linked pin | Event processing shares a cached time; rendering uses predicted presentation time, with a separately adjusted animation clock | Consistent timing does not establish identical latency or refresh behavior in another compositor | [Timing contract](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Development:-Animation-Timing.md#L26) |
| Swayfx @ 0.6 | A closing container retains its saved client buffer until animation completion | The closing transition starts from its interpolated size and opacity | [Closing lifetime](https://github.com/wlrfx/swayfx/blob/0.6/sway/desktop/transaction.c#L94) |

- Ash skips its bounds animation when the window layer's own [target transform is nonidentity](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L1147). Its [crossfade path](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_state.cc#L1229) recreates the layer tree and animates retained old layers after resizing. This guard concerns that window layer; the source does not establish an equivalent guard for arbitrary CSS ancestors. [source]

- Aura Hide immediately clears window visibility even when layer painting continues for an animation. Final event acceptance permits ending events after EventClient authorization before applying the visibility check. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window.cc#L480), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window.cc#L1969).

- The layer-animation observer counts successful and aborted sequences separately; its ended callback can run after all sequences detach. Sequence progression guards against callbacks deleting the delegate. These are animation lifecycle signals, separate from display presentation. [source] ([Observer conditions](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/callback_layer_animation_observer.cc#L114), [Sequence lifetime](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer_animation_sequence.cc#L82))

- Ash: When a top-level window owns its layer, Ash teardown completes its animations before deletion and rechecks a tracker because completion can delete the window. [source] ([Lifecycle source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/root_window_controller.cc#L787)).

- LayerAnimator’s [immediate policies](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer_animator.cc#L704) distinguish progressing conflicting sequences to their ends from aborting them and starting the new sequence. Completion and abort callbacks consequently describe different transitions; the policy names alone establish no velocity-continuity contract. [source]

- During ordinary current-state tracking, Aura’s [occlusion tracker treats animated visible subtrees as unoccluded](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window_occlusion_tracker.cc#L501) and excludes their contribution to coverage. Transform, bounds, opacity and color animations participate; [completion or abortion triggers recomputation](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window_occlusion_tracker.cc#L987). The rule is scoped to Aura-window tracking; [target-value prediction uses a separate path](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window_occlusion_tracker.cc#L668). [source]

- [LayerOwner recreation](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer_owner.cc#L61) moves children and the painting delegate into a clone and returns the old layer. [Recursive recreation](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/window_util.cc#L183) retains owned branches, skipping a branch without an owner or mapped layer. [source]
- A base [layer clone copies target properties and backdrop settings](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.cc#L262). A base [paint mirror forwards to the source’s current delegate](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer_mirror.cc#L14), when present; destroying the relationship removes the destination’s delegate and observer. These contracts distinguish retained layers, copied state and delegated painting. [source]


- ChromeOS-gated Exo defers host-origin updates and suppresses the new widget shadow while client commits target a different cloned layer; its source leaves updating the old shadow unresolved. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L2066), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L2120). [Exo target gate](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/BUILD.gn#L13).


- ChromeOS-gated Ash mirror-view layer recreation moves its existing mirror-tree root under the returned old view layer for close animation, then initializes a new mirror tree while its source remains available. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_mirror_view.cc#L122).

- ChromeOS-gated Exo uses a timed compositor lock during maximized, fullscreen or pinned transitions when a configure callback exists; without that callback it temporarily disables the default transition animation. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface.cc#L754), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface.cc#L785).

- ChromeOS-gated Exo selects current or older commit-target layers by LocalSurfaceId parent sequence; unmatched stale sequences return no target. Surface fallback ranges stretch content until the client sequence catches up. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface.cc#L495), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface.cc#L1176).

- Generic Layer distinguishes movement from resizing: movement redraws only while visible; resizing recomputes backdrop bounds and schedules paint even while invisible. The layer type still controls whether a paint request is accepted. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.cc#L1356), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.h#L449).

- Underlying layer replacement requires transform and opacity animations to finish first; that helper checks weak lifetime because observers may delete the wrapper. Replacement then copies compositor properties and reattaches child layers. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.cc#L937), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.h#L648).

- Surface-backed layer cloning reuses the source SurfaceId and copies deadline, size, stretch and fallback settings. External-texture mirroring instead reuses a transferable resource with a no-op mirror release callback, leaving release authority with the source. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer_surface.cc#L48), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer_with_external_texture.cc#L91).

## What the notes found

- Window-scene animation and compositor ownership are distinct from choosing Views or a web frontend. [inferred]
- A source-level resemblance does not establish matching input, restoration or cancellation semantics. [inferred]
- 2026-11-09: Exo's vendor blending object restores source-over compositing and alpha 1 for a surviving surface when destroyed. Its factory rejects a second blending object on the same surface (exo@150.0.7871.124) [source] <https://github.com/chromium/chromium/blob/150.0.7871.124/components/exo/wayland/zcr_alpha_compositing.cc#L40> <https://github.com/chromium/chromium/blob/150.0.7871.124/components/exo/wayland/zcr_alpha_compositing.cc#L108>
- 2026-11-09: Scroll separates clip-style resize from content stretching and gives changing-variable and off-axis motion independent curves; optional simple cubic timing follows CSS conventions (scroll@b6586d273fed7d187744b7eaec2c234386a029fa) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L75> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L136>
- 2026-11-08: KWin retains closed-window state through an effect reference guard, and spring retargeting preserves position and velocity while changing the target anchor (KDE/KWin@0406762df7794213f12ce87bc8f6fd322dcdc53c) [source] <https://github.com/KDE/kwin/blob/0406762df7794213f12ce87bc8f6fd322dcdc53c/src/effect/effectwindow.h#L841> <https://github.com/KDE/kwin/blob/0406762df7794213f12ce87bc8f6fd322dcdc53c/src/effect/springmotion.cpp#L118>
- 2026-11-09: Niri’s timing model caches one timestamp per event iteration and overrides it with predicted presentation time for rendering; animation slowdown applies through a separate adjusted clock (niri@90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb) [source] <https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Development:-Animation-Timing.md#L26>
- 2026-10-06: A 2026-10-06 source comparison records that Views compositor opacity and transform animation avoids repainting while its single-threaded cc schedule remains on the UI thread (Chromium 154) [docs] <https://github.com/chromium/chromium/blob/154.0.8037.92/ui/compositor/compositor.cc#L255>
- 2026-10-22: The source comparison found closing-animation lifetime changes deeper than effect-node setup; destruction waits for the close animation to finish (swayfx@0.6) [docs] <https://github.com/wlrfx/swayfx/blob/0.6/sway/desktop/transaction.c>
- 2026-08-05: Swayfx 0.6 was published on 2026-08-05; its release notes identify Sway 1.12, wlroots 0.20 and SceneFX 0.5, alongside move, open/close and workspace animation changes (swayfx@0.6) [docs] <https://github.com/wlrfx/swayfx/releases/tag/0.6>
- 2026-10-06: The native-view survey distinguishes retargeted layer-bounds animation from tab-strip layout interpolation on the UI thread; a shared easing label does not imply the same resize mechanism (Electron and Chromium native views, 2026-10-06 survey) [docs]
- 2026-11-06: Domicile retained the last displayed surface after a normal close while the shell kept referencing it; closing and temporarily unmapping follow different native paths, and no presentability notification contract was identified [docs]

## Where it touches Domicile

- `manganese`: motion state retains closed windows during render, avoiding an intermediate unmount of their guest. Animation end events release retained state. [Retained animation state](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindowMotion.ts#L171). [source]
- `manganese`: overview uses CSS transforms and clipping wrappers. A transition permits reversal while opening; wrapper clipping preserves monitor boundaries independently of the transform. [Overview transform and clip](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/overview/seen-styles.ts#L8). [source]
- Status at the pinned checkout: `landed`; closing and workspace animation behavior is present in source. Public changes: [cprussin/domicile#1910](https://github.com/cprussin/domicile/pull/1910), [#1939](https://github.com/cprussin/domicile/pull/1939), [#1940](https://github.com/cprussin/domicile/pull/1940). [source]
- Donor animation policy lives above the layer mechanism; the timing and event semantics remain the shell's contract. [inferred]

- A browser guest sets its [resizing flag during settling and workspace slide-in](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/BrowserWindow.tsx#L753), allowing the transition to proceed while its page redraws. This is separate from the native client's configure/commit path. [source]

- Native close [removes its active mapping and returns unused waiting frames](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/engine_session.rs#L725) while retaining surface and import identities for later cleanup. A valid linger completion forgets imports, destroys the surface and ends remaining hold tracking. The [linger helper](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/engine_lingering.rs#L38) checks a generation token so an old completion cannot remove a newer linger using the same surface identity. [source] These ranges establish deferred cleanup and stale-completion protection, leaving timer scheduling and visible animation unqualified. [inferred]

## Lineage

- The July lift map separated compositor animation machinery from Ash window policy.

## Sources

- S-502e32dd38c7 · 2026-11-09 · Pinned window animation lifetime
- S-e19473fe102a · 2026-11-09 · Pinned overview transform and clip
- S-e2a96179b3b7 · 2026-11-09 · Pinned native-window element behavior
- S-14b41dc319ce · 2026-11-09 · Domicile browser-guest motion and redraw-wait policy
- S-7cb0007051a1 · 2026-11-08 · KWin closed-window effect reference contract
- S-4a9f3490b0b6 · 2026-11-08 · KWin effect lifetime and retained window state
- S-6bec7a90d975 · 2026-11-08 · KWin spring retargeting state
- S-70ac6562eb03 · 2026-11-09 · Niri event and predicted-presentation animation clocks
- S-de92af3f5a9b · 2026-11-09 · Swayfx closing buffer retention and interpolation
- S-6da8864cbb54 · 2026-11-09 · Ash own-layer transform guard and crossfade retention
- S-578852a49413 · 2026-11-09 · Aura window visibility and event completion
- S-7e62b17cb4f1 · 2026-11-09 · Chromium animation observer completion and abort conditions
- S-6b5cc0eed377 · 2026-11-09 · Chromium animation sequence redraw and callback lifetime
- S-af414fdb64d0 · 2026-11-09 · Ash child-layer animation completion and destruction guard
- S-a6ccc227ff5c · 2026-11-09 · Chromium animation preemption, queued replacement and start groups
- S-acf45a44d844 · 2026-11-09 · Aura window occlusion, animation and declared coverage
- S-581a3e7127e2 · 2026-11-09 · Chromium owned-layer replacement and delegate transfer
- S-83c6a5bbb592 · 2026-11-09 · Chromium layer target-state cloning
- S-8191329f44ce · 2026-11-09 · Chromium paint-mirror delegate lifetime
- S-55c701699898 · 2026-11-09 · Chromium recursive retained-layer ownership
- S-be5932a4f7a2 · 2026-11-09 · Exo shell-surface shape, commit and popup lifecycle
- S-4111d1fe471f · 2026-11-09 · Exo build-target gate
- S-c3368d7ab705 · 2026-11-09 · Ash state, display-root or preview implementation
- S-4a6520490f9a · 2026-11-09 pin check · Chromium overview transform and restoration interface
- S-1bcd456a94e6 · 2026-11-09 pin check · Chromium 150 animation ownership and preemption interface
- S-2f2973c4cda8 · 2026-11-09 · Exo configure, commit-target, transition and transient-parent paths
- S-de6569bcc761 · 2026-11-09 · Chromium layer property, content ownership and lifetime contract
- S-85179425f20e · 2026-11-09 · Chromium layer property, content ownership and lifetime contract
- S-cd1972571051 · 2026-11-09 · Chromium layer property, content ownership and lifetime contract
- S-963f6f0016cd · 2026-11-09 · Domicile native session close and deferred cleanup
- S-e34b121eace4 · 2026-11-09 · Domicile native linger generations and stale-completion guard
