---
id: 00-overtures/blur
title: "Blur across compositor and page boundaries"
kind: overture
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-8f65c1138fcf, S-398d594ace72, S-01664694e420, S-1db325237c9a, S-bdce45361e59, S-7b28df8be83d, S-fc903ad084e3, S-2f963e0d8b20, S-5b1bd336dbff, S-6a02b449627e, S-83c6a5bbb592, S-de6569bcc761, S-3c38ec8fb4c4, S-ce1db11e9725, S-e463b7863c97, S-72b0299e65dc, S-b9554b72a40c, S-703c0d408b29, S-292e57b07af7, S-ab41375a77ca, S-d515a1151293, S-b684634e239b, S-5f2e276eeb55, S-b7a117c39f0d, S-7eefe75063cb, S-fbdf04224fc9, S-8100b35ed50a]
---

# Blur across compositor and page boundaries

Blur depends on which scene the effect can sample. A window-local filter, a layer backdrop and a compositor's scene below a surface have different inputs. The earlier studies explored all three. [docs]

## Compositor-side filtering

The Swayfx/SceneFX study separates blur passes, cached source sampling and damage expansion. Its [blur configuration](https://github.com/wlrfx/scenefx/blob/0.5/types/fx/blur_data.c#L25) describes a compositor donor. [docs]

SceneFX's paired [downsample kernel](https://github.com/wlrfx/scenefx/blob/0.5/render/fx_renderer/shaders/blur1.frag#L9) and [upsample kernel](https://github.com/wlrfx/scenefx/blob/0.5/render/fx_renderer/shaders/blur2.frag#L9) filter a supplied texture at two sampling scales. The [caller](https://github.com/wlrfx/scenefx/blob/0.5/render/fx_renderer/fx_pass.c#L978) expands damage, alternates intermediate buffers and runs both filter loops. Its [optimized path](https://github.com/wlrfx/scenefx/blob/0.5/render/fx_renderer/fx_pass.c#L1101) can reuse a saved result; reduced strength can instead refilter the saved unblurred source. These mechanisms do not supply a runtime cost comparison or establish every surface's cache eligibility. [source]

The [scene blur node](https://github.com/wlrfx/scenefx/blob/0.5/types/scene/wlr_scene.c#L2228) explicitly selects that cache through its bottom-layer flag. A [dirty optimized node](https://github.com/wlrfx/scenefx/blob/0.5/types/scene/wlr_scene.c#L2162) refreshes when blur work is present and clears its dirty flag after successful rendering. [source]

Hyprland's [blur provider](https://github.com/hyprwm/Hyprland/blob/64bde62ee467d7fe56d15431e91eaa4bb47ddd14/src/render/blur/Provider.hpp#L25) includes dual Kawase and exposes framebuffer filtering and damage expansion. The cited interface does not establish which provider every configuration selects. [source]

KWin's [downsample pass](https://github.com/KDE/kwin/blob/580a077e94f35c73417de9e880508f1c0b5d8dbd/src/plugins/blur/blur.cpp#L804) and [upsample reconstruction](https://github.com/KDE/kwin/blob/580a077e94f35c73417de9e880508f1c0b5d8dbd/src/plugins/blur/blur.cpp#L831) implement its dual-Kawase path. Blur strength also determines the sampled area outside the visible region. [source]

## Niri under web shells

In the COWL client era, the web shell drew translucent foreground content and requested a blur region. Niri owned the scene and filter settings. The notes distinguish wallpaper-only cached sampling from sampling the actual windows below the shell surface. [docs]

The old CSS limitation belonged to that boundary: CSS inside a client panel could sample its own page, not the unrelated Wayland desktop behind its native surface. That account does not settle a compositor whose page contains application surfaces. [inferred]

## Aura layer backdrop filters

Chromium's [Layer blur setter](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.h#L270), [backdrop bounds](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.h#L349) and [filter-quality setter](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.h#L520) expose blur as layer properties. Explicit bounds are layer-local and constrain both the visible effect and sampled input to their intersection with the layer. The [implementation](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.cc#L792) recomputes default layer bounds after an explicit override is cleared; this does not disable blur. Rounded output clipping is separate from the sampled rectangular region. Quality affects the temporary filtering surface. This is reusable engine machinery rather than an Exo protocol server. [source]

A [backdrop-filter render-surface reason](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/cc/trees/property_tree_builder.cc#L356) is assigned when backdrop filters are present. Expressing blur as a layer property still entails intermediate composition structure. [source]

An intermediate composition pass does not by itself establish an extra display-frame stage. The two costs require different evidence. [inferred]

The [blur setter](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.cc#L570) directly rebuilds filters. The stock [animatable-property enum](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer_animation_element.h#L35) has no blur-sigma entry. [source]

Setter availability therefore does not itself provide implicit sigma animation. This does not determine Blink CSS behavior or what a custom caller can update. [inferred]

## Ash background shields

Ash's [background shield](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/style/blurred_background_shield.cc#L111) applies a backdrop blur to a host layer. Its [style constants](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/style/style_util.h#L52) retain sigma 30 and quality 0.33 at the engine pin. The [layer implementation](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.cc#L1006) divides requested quality by device scale before forwarding it; the API describes a possible temporary filtering surface. Those defaults are source facts, not performance measurements. Ash remains ChromeOS-gated; the composition is a donor pattern. [source]

## Background-effect protocol

The [versioned protocol](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.48/staging/ext-background-effect/ext-background-effect-v1.xml) carries a surface-local clipped region with copied, double-buffered state. It leaves the algorithm to compositor policy. An empty or null blur region removes the effect; capability changes can stop it. [source]

## Miracle-wm focus blur

The [versioned Miracle-wm plugin documentation](https://wiki.miracle-wm.org/v0.11.2/plugins/) lists a focus-blur example for unfocused windows. The inspected donor snapshot changes an unfocused window's own image and clears the shader on focus; attached tiled windows are excluded in that snapshot. This is content filtering, distinct from sampling behind the window. The wiki establishes the example's purpose, while the snapshot supplies these finer behavioral details. [docs]

## Otto Skia validation

The Otto validation notes record adapting KDE blur requests to an existing backdrop path. The adapter preserved a semantic difference: an empty KDE region meant the whole surface, while an empty standard background-effect region disabled blur. The reported visual checks are historical notes, not a new Domicile measurement. [docs]

## Implementation correspondence

The material study's renderer analogy does not establish implementation equivalence. [docs]

## CSS and page-scene effects

Domicile's [application-layer implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/layout_app_surface.cc#L57) places the foreign layer in page property trees, where ordinary CSS transform, clip, opacity, filter and blend apply. [source]

The [window compositing account](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md) documents that page scene. This changes the old separate-panel boundary; CSS filtering may supersede a shell-specific blur protocol where the required application content is sampled. [inferred]

A three.js shader can process textures available to its scene; the notes do not establish an API exposing arbitrary native application surfaces as those textures. [inferred]

Chromium's [underlay check](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/viz/service/display/overlay_strategy_underlay.cc#L55) rejects an ordinary candidate intersecting filtered content above it; its [aggregate candidate check](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/viz/service/display/overlay_candidate_factory.cc#L556) rejects backdrop-filtered candidates. These checks establish promotion limits for the affected content; required overlays retain a separate exception. [source]

The [known gaps](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md) distinguish protected surfaces that require overlays. The lit-display filter result remains a hardware qualification boundary. [docs]

The remaining sampling question is whether CSS `backdrop-filter` includes native `<app>` surface layers on the lit display path at the checked pin; [the feature page](../06-features/effects/backdrop-blur.md) retains that boundary. [inferred]

## Sources

- S-292e57b07af7 · 2026-10-22 · Swayfx filtering, scene sampling and damage study
- S-ab41375a77ca · 2026-11-09 · SceneFX blur defaults and damage-expansion source
- S-d515a1151293 · 2026-04-04 · Niri-era client-region and compositor-filter contract
- S-b684634e239b · 2026-07-18 · Chromium layer backdrop-filter inventory
- S-5f2e276eeb55 · 2026-07-18 · Ash shield and wallpaper-effect inventory
- S-b7a117c39f0d · 2026-11-09 · Pinned focus-blur donor inspection
- S-7eefe75063cb · 2026-10-24 · Otto backdrop adapter and validation account
- S-fbdf04224fc9 · 2026-04-10 · Material and layer-composition study
- S-8100b35ed50a · 2026-11-09 · Domicile rendering qualification boundaries
- S-de6569bcc761 · 2026-11-09 · Chromium layer backdrop bounds and filter quality
- S-3c38ec8fb4c4 · 2026-11-09 · Ash background shield blur calls
- S-ce1db11e9725 · 2026-11-09 · Ash blur sigma and filtering quality constants
- S-e463b7863c97 · 2026-11-09 · Versioned surface background-effect protocol
- S-72b0299e65dc · 2026-11-09 · KWin downsampling and upsampling blur path
- S-b9554b72a40c · 2026-11-09 · Hyprland blur provider and damage interface
- S-703c0d408b29 · 2026-11-09 · Domicile application surfaces in page composition
- S-83c6a5bbb592 · 2026-11-09 · Chromium default backdrop sampling bounds and rounded output clipping
- S-6a02b449627e · 2026-11-09 · Chromium backdrop-filter intermediate render-surface reason
- S-5b1bd336dbff · 2026-11-09 · Domicile native-application layer in page property trees
- S-2f963e0d8b20 · 2026-11-09 · Chromium stock layer-animation property enumeration
- S-fc903ad084e3 · 2026-11-09 · Versioned Miracle-wm plugin documentation and focus-blur example
- S-bdce45361e59 · 2026-11-09 · Chromium intersecting-filter underlay rejection
- S-7b28df8be83d · 2026-11-09 · Chromium backdrop-filter aggregate candidate rejection
- S-01664694e420 · 2026-11-09 pin check · SceneFX downsample texture-filter kernel
- S-1db325237c9a · 2026-11-09 pin check · SceneFX upsample texture-filter kernel
- S-398d594ace72 · 2026-06-28 · SceneFX filter loops, intermediate buffers and conditional cache reuse
- S-8f65c1138fcf · 2026-06-28 · SceneFX scene-node cache selection, refresh and damage conditions
