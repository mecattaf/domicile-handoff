---
id: effects/backdrop-blur
title: Background blur behind windows and shell surfaces
kind: feature
family: effects
layers: [engine:blink, engine:viz, any-shell]
domicile_status: known-gap
domicile_refs: [docs/architecture/ENGINE-FORK.md, docs/architecture/WINDOW-COMPOSITING.md, docs/architecture/ENGINE-FORK-MEASUREMENTS.md, docs/KNOWN-GAPS.md]
checked_at: "2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc"
answered_upstream: false
scroll_parity: n/a
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:effects, status:known-gap, bucket:domicile, evidence:source]
sources: [S-17b9826676ac, S-1c5da8dc640a, S-292e57b07af7, S-2fcac3a67284, S-30fcc2cefcdf, S-4a468ec61206, S-514bff6c69a0, S-518da8290a0a, S-6e22344c1e7f, S-703c0d408b29, S-7b28df8be83d, S-8100b35ed50a, S-83c6a5bbb592, S-8cc20f695fcd, S-93bfe16ec4f6, S-9b6eacc885db, S-ab41375a77ca, S-bdce45361e59, S-c153d333f325, S-d4a790b4dc24, S-de6569bcc761, S-f8f2f5e99161, S-fcbbb5e495a6]
donor_availability: in-engine-build
donors: [aura, ash, kitty]
eras: [Aura lift (July 2026), views-shell]
rows: [AL-ash-overview-023, AL-ash-sysui-blur-wallpaper-001, AL-ash-sysui-blur-wallpaper-002, AL-ash-sysui-blur-wallpaper-009, AL-ash-sysui-blur-wallpaper-010, AL-ash-sysui-blur-wallpaper-011, AL-ash-sysui-blur-wallpaper-012, AL-ash-sysui-blur-wallpaper-013, AL-ash-sysui-blur-wallpaper-014, AL-ash-sysui-blur-wallpaper-015, AL-ash-sysui-blur-wallpaper-016, AL-ash-sysui-blur-wallpaper-017, AL-compositor-blur-anim-001, AL-compositor-blur-anim-002, AL-compositor-blur-anim-003, AL-compositor-blur-anim-004, AL-compositor-blur-anim-006, AL-compositor-blur-anim-007, AL-cade-triage-035, AL-cade-triage-040, AL-cade-triage-104, AL-cade-triage-107, AL-cade-triage-112, AL-cade-triage-113, AL-cade-triage-201, AL-cade-decision-022, AL-cade-decision-027, AL-cade-decision-091, AL-cade-decision-094, AL-cade-decision-099, AL-cade-decision-100, AL-cade-decision-188, AL-cubs-aura-exo-005, AL-cade-machinery-wm-009, AL-cade-machinery-wm-013, AL-cade-spec-coverage-004, AL-cade-niri-coverage-079, AL-machinery-central-016, AL-april-protocol-triage-018, AL-april-protocol-triage-037, AL-april-protocol-triage-053, AL-april-reverse-triage-054, AL-april-reverse-triage-063, AL-aura-vs-mir-002, AL-aura-vs-mir-029, AL-aura-vs-mir-056, AL-aura-vs-mir-058, AL-surface-matrix-028, AL-surface-matrix-029, AL-engine-check-001, AL-engine-check-009, AL-engine-check-019]
depends_on: []
reported_on_domicile: []
---
# Background blur behind windows and shell surfaces

A translucent shell element can filter the scene behind it. Native client pixels and page content meet in Chromium's scene graph; hardware overlays define a separate validation boundary. [docs]

## Behavior

- A backdrop filter samples content below an element; a foreground `filter: blur()` blurs the element itself.
- Clipping, bounds, transforms and occlusion change the sampled region.
- Protected content can impose a different overlay requirement. [docs]

## How donors do it

| Donor @ pin | Mechanism | Boundary | Link |
|---|---|---|---|
| Aura @ Chromium 150 | Layer background blur and backdrop bounds | Engine-layer effect; availability of its caller is separate | [Layer interface](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ui/compositor/layer.h) |
| Ash @ Chromium 150 | BlurredBackgroundShield | ChromeOS-gated system UI uses an Aura layer | [Shield](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/style/blurred_background_shield.cc) |

- Viz’s damage prewalk conservatively [expands intersecting damage under a pixel-moving backdrop filter to its render-pass quad rectangle](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/viz/service/display/surface_aggregator.cc#L1854). Foreground filters use a separate expansion path. This establishes damage accounting in the donor, leaving runtime timing and copy counts unmeasured. [source]

- The ChromeOS-gated [shield implementation](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/style/blurred_background_shield.cc#L103) enables background blur only for a translucent resolved color and nonzero sigma. It otherwise clears the blur; the wrapper also tracks host visibility and bounds. These are donor conditions, without a Domicile runtime qualification. [source]

- Generic Layer divides requested backdrop-filter quality by device scale before forwarding it. Its API describes quality as the scale of a possible temporary filtering surface; this is a parameter contract, not observed performance. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.cc#L1006), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.h#L514).

- The checked [Kitty client path](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_window.c#L347) requests a background-effect blur region when blur capability is advertised. Its KDE fallback requires absence of the standard manager. The [native setter](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_window.c#L3033) enables the blur flag for transparent-window mode and a positive radius. [source] These are external-client request conditions; compositor sampling remains a separate boundary. [inferred]

## What the notes found

- The layer-shell study distinguishes a client's blur request from a compositor that can sample the scene behind that client. S-17b9826676ac. [docs]
- The [blur overture](../../00-overtures/blur.md) keeps compositor filters, material treatments and the web-engine path in one chronology.
- 2026-07-18: Kitty requests a background-effect blur region only when the compositor advertises blur capability. If that manager is unavailable, it can use the KDE blur protocol (kitty@0.48.0) [source] <https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_window.c#L347> <https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_window.c#L365>
- 2026-07-18: Kitty’s native blur setter returns without requesting blur for an opaque window. With a transparent window, a positive blur radius enables the flag used by its protocol-region path (kitty@0.48.0) [source] <https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_window.c#L3033> <https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_window.c#L347>
- 2026-11-04: Native buffer alpha is preserved: formats with alpha are blended and opaque formats marked opaque, so a translucent terminal over a filtered background keeps sharp text; whole-window CSS opacity on an opaque client also fades its text [source] <https://github.com/cprussin/domicile/blob/08f0687dafa1018a9dcd7f227f43cc6dc557ffec/packages/domicile-engine/src/components/domicile/engine/surface_alpha.h> <https://github.com/cprussin/domicile/blob/08f0687dafa1018a9dcd7f227f43cc6dc557ffec/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L283>
- 2026-11-09: Kitty binds ext_background_effect_manager_v1 when advertised and installs its capability listener; its separate KDE blur binding is another optional registry path (kitty@0.48.0) [source] <https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_init.c#L699>
- 2026-11-04: The intended backdrop path is Viz SkiaRenderer filtering the render pass that already contains the native window quad; the native surface enters Blink property trees via LayoutAppSurface::PaintReplaced and RecordForeignLayer [source] <https://github.com/cprussin/domicile/blob/08f0687dafa1018a9dcd7f227f43cc6dc557ffec/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/layout_app_surface.cc>
- 2026-11-04: Hyprland blur exposes size and passes, opacity handling, noise, brightness, contrast, vibrancy, xray and popup handling; its acrylic material adds refraction, chromatic separation, tint and a sharp-backdrop term; GTK4 clients request blur through the background-effect protocol (hyprland@64bde62ee467d7fe56d15431e91eaa4bb47ddd14) [docs] <https://wiki.hypr.land/configuring/core/config-options/#blur> <https://wiki.hypr.land/faq/#blur>
- 2026-10-22: The blur source study separated renderer passes, scene nodes, expanded damage and cached background sampling from the command grammar (swayfx@0.6) [docs] <https://github.com/wlrfx/scenefx/blob/0.5/types/fx/blur_data.c#L25>
- 2026-03-15: A public March discussion separates static background filtering from behind-window blur and describes the latter as requiring additional renderer passes; it does not report a delivered Scroll blur implementation (scroll) [docs] <https://github.com/dawsers/scroll/discussions/229#discussioncomment-16182126>
- 2026-07-08: SceneFX 0.5 was published on 2026-07-08; that release date identifies the versioned blur-damage reference and does not date every behavior's introduction (scenefx@0.5) [docs] <https://github.com/wlrfx/scenefx/releases/tag/0.5>
- 2026-11-04: At the 2026-11-04 checkout, upstream docs report GPU pixel parity for filter: blur() on an `<app>` element, with zero differing pixels above threshold across the test cells; rounded clip, opacity, transform and blend parity are reported alongside [docs] <https://github.com/cprussin/domicile/blob/08f0687dafa1018a9dcd7f227f43cc6dc557ffec/docs/architecture/ENGINE-FORK-MEASUREMENTS.md#css-parity>
- 2026-11-04: At the 2026-11-04 checkout, backdrop-filter blur on shell chrome over an `<app>` is rendered, but upstream marks its pixel verification and presentation on a lit display as pending [docs] <https://github.com/cprussin/domicile/blob/08f0687dafa1018a9dcd7f227f43cc6dc557ffec/docs/architecture/WINDOW-COMPOSITING.md#backdrop-filter-over-a-window>
- 2026-11-04: Hardware overlay promotion beneath a backdrop-filter quad is rejected by Viz guards (protected content excepted); upstream notes trace these guards from source and have not validated them against a lit display [docs] <https://github.com/cprussin/domicile/blob/08f0687dafa1018a9dcd7f227f43cc6dc557ffec/docs/architecture/ENGINE-FORK-CHROMIUM-NOTES.md#backdrop-filters-and-overlay-promotion>
- 2026-11-04: SwayFX exposes blur radius and passes plus noise, brightness, contrast and saturation settings, and a restricted set of layer-shell effects; it documents background-only blur for floating windows (swayfx) [docs] <https://github.com/wlrfx/swayfx#new-configuration-options>
- 2026-11-04: Otto accepts per-surface blur-region and vibrancy requests through a surface-style protocol; its blur, saturation, contrast and tint recipe may be expressible as CSS backdrop-filter without a shader port, which was not tested (otto@unpinned) [inferred] <https://github.com/nongio/otto/blob/main/docs/developer/surface-style-protocol.md>
- 2026-10-06: An October 2026 shell-surface study records ext-background-effect alongside background opacity and blur [docs]

## Where it touches Domicile

- `engine:blink`: `<app>` uses a surface layer in the same CSS property trees as other page elements. [Engine boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md#L3). [docs]
- `engine:viz`: the documented renderer applies the backdrop filter to a render pass containing the client quad. Backdrop filtering can prevent ordinary overlay promotion for affected, intersecting content; required overlays have a separate exception. [Backdrop filter boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L269). [docs]
- The pinned [underlay check](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/viz/service/display/overlay_strategy_underlay.cc#L55) rejects non-required candidates intersecting filtered content above them. The [aggregate-candidate check](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/viz/service/display/overlay_candidate_factory.cc#L556) also rejects candidates carrying backdrop filters. Hardware planes combine at display presentation, beyond framebuffer filter sampling. These checks constrain promotion without establishing a Domicile display result. [source]
- Status at the pinned checkout: `known-gap` records the retained backdrop-sampling and presentation verification gap. Foreground CSS blur has a published pixel comparison; backdrop blur and overlay behavior remain unmeasured on a lit display. [Pixel comparison](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK-MEASUREMENTS.md#L61); [Known presentation gap](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md#L30). [docs]
- The layer boundary is between CSS scene effects and viz presentation. The pinned documents do not establish that an extra Wayland blur protocol is required for shell-owned page elements. [inferred]

- Native buffer alpha and whole-element opacity have different inputs. The current helper recognizes ARGB8888 and ABGR8888 alpha; the import and frame callers carry that into the contents-opaque marker and blending eligibility while quad opacity stays one. This buffer classification is independent of fading the entire page element. [source] [Alpha classification](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/surface_alpha.cc#L8), [native quad flags](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L295).


## Lineage

- The July Aura study cataloged engine-layer blur and Ash wrappers.
- The October client-shell census retained the external compositor as the effect provider.

## Sources

- S-17b9826676ac · 2026-10-01 · Chromium window-system and shell seam census
- S-514bff6c69a0 · 2026-11-09 · Pinned engine surface architecture
- S-703c0d408b29 · 2026-11-09 · Pinned native-window composition boundaries
- S-8100b35ed50a · 2026-11-09 · Pinned presentation limits
- S-1c5da8dc640a · 2026-11-09 · Published CSS pixel comparisons
- S-30fcc2cefcdf · 2026-11-09 · Pinned native-buffer alpha contract
- S-bdce45361e59 · 2026-11-09 · Chromium filtered-content underlay eligibility
- S-7b28df8be83d · 2026-11-09 · Chromium backdrop-filter aggregate candidate eligibility
- S-4a468ec61206 · 2026-11-09 · Domicile recognized alpha formats
- S-c153d333f325 · 2026-11-09 · Domicile native import, frame construction and feedback routing
- S-8cc20f695fcd · 2026-11-09 · Chromium surface aggregation, backdrop damage and pass merging
- S-d4a790b4dc24 · 2026-11-09 pin check · Chromium 150 translucent shield and below-host layer lifecycle
- S-6e22344c1e7f · 2026-11-09 pin check · Chromium 150 layer backdrop sampling and clip contract
- S-83c6a5bbb592 · 2026-11-09 · Chromium layer property, content ownership and lifetime contract
- S-de6569bcc761 · 2026-11-09 · Chromium layer property, content ownership and lifetime contract
- S-93bfe16ec4f6 · 2026-11-09 pin check · Kitty 0.48 client blur capability, fallback and setter conditions
