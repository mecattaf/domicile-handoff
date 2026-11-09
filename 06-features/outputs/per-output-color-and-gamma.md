---
id: outputs/per-output-color-and-gamma
title: Per-output night light and gamma ramps
kind: feature
family: outputs
layers: [engine:drm, compositor, protocol, sdk]
domicile_status: landed-with-gap
domicile_refs: [cprussin/domicile#1915, cprussin/domicile#1921]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: true
scroll_parity: baseline
donors: [sway, scroll, kde]
rows: [color-management-v1, color-representation-v1, wlr-gamma-control-v1]
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:outputs, status:landed-with-gap, evidence:source, scroll-parity:baseline]
sources: [S-2637566bae64, S-307b22e8e1f5, S-43af2b3af183, S-45b621140a35, S-497068d45989, S-5a9e3b32e108, S-612a99e2b687, S-6f7abf0045c1, S-74f557738f56, S-7b28df8be83d, S-7cd85cb7ef61, S-8100b35ed50a, S-8f65c1138fcf, S-93f31cb66e29, S-951964426c98, S-b59590b62aaa, S-cf898850da22, S-e7392f226f69, S-ec0011587fbd, S-f0e52c92dd4e]
---
# Per-output night light and gamma ramps

Each monitor has its own gamma ramp. A night-light temperature supplies the default ramp, while a gamma-control client can own one monitor independently. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L266) [docs]

## Behavior

- A client ramp temporarily replaces night light on that monitor. Releasing the client restores the night-light ramp; the effects are not multiplied. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L276) [docs]
- The neutral temperature is 6500 K and produces an identity ramp. Hardware ramp size determines the client buffer length. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L291) [docs]
- A second ramp owner, missing hardware ramp or removed monitor receives protocol failure. DisplayLink, nested and headless outputs have no hardware ramp. Nested display control belongs to the host. Sandboxed clients do not see the gamma global. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L282) [docs]

- A stored manual night-light override takes precedence over the schedule. With no schedule, it has no scheduled expiry; disabling scheduling does not clear that override. [source] [Night-light state](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-bar-module-brightness/src/night-light.ts#L69)

- Named night-light overrides survive output removal in compositor memory. An all-output temperature call clears them. Manganese uses that all-output form when its computed temperature changes. [source] [Named and global values](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/gamma.rs#L70) · [Night-light caller](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-bar-module-brightness/src/desk-night-light.ts#L16)
- Reserving a gamma-control object leaves night light active until the first client ramp arrives. Releasing the client restores the named value for that monitor, or the global value. [source] [Client ownership](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/gamma.rs#L148)

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Exposes the wlr gamma-control manager to clients | gammastep-style clients can set an output ramp | [source](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/server.c#L330) |
| scroll | Retains the sway protocol surface | A per-output gamma client remains independent of layout | [source](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/server.c#L333) |

## What the notes found

- The useful distinction is between warming/dimming with a hardware gamma ramp and describing a color-managed surface. The former does not establish ICC, wide-gamut or HDR support. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L293) [inferred]
- 2026-11-09: Sway and Scroll enable HDR output descriptions only with reported BT2020 primaries, PQ transfer support and renderer color transforms. They disable HDR when a color profile is configured (sway/scroll pinned references) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L351> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L557> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L407> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L651>
- 2026-11-09: Sway and Scroll default render depth to ten bits for HDR requests and eight otherwise, unless explicitly set. Sway’s fallback search tries supported render formats up to the requested depth; hardware acceptance remains separate (sway/scroll pinned references) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L464> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config/output.c#L520> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/config/output.c#L898>
- 2026-11-09: Ash NightLightController exposes a color-temperature setter and depends on Ash display and session controllers; API similarity does not establish Linux build availability (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/system/night_light/night_light_controller_impl.h#L111> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/system/night_light/night_light_controller_impl.cc#L327>
- 2026-11-09: Ash’s ChromeOS-gated night light tries each display’s hardware CTM and falls back to its compositor color matrix; successful hardware adjustment resets the compositor matrix to identity to avoid applying the effect twice (ash@domicile-engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/system/night_light/night_light_controller_impl.cc#L147> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/system/night_light/night_light_controller_impl.cc#L177>
- 2026-11-09: Ambient color adjustment applies only to internal displays. The unified virtual host is skipped because its physical mirroring hosts already receive the transform; connected displays are handled individually (ash@domicile-engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/system/night_light/night_light_controller_impl.cc#L177> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/system/night_light/night_light_controller_impl.cc#L210>
- 2026-11-09: SceneFX 0.5 renders a gamma LUT when hardware gamma size is zero and the renderer advertises output-color transforms; its direct-scanout attempt excludes that rendered-gamma path (SceneFX@0.5) [source] <https://github.com/wlrfx/scenefx/blob/0.5/types/scene/wlr_scene.c#L3085> <https://github.com/wlrfx/scenefx/blob/0.5/types/scene/wlr_scene.c#L3190>
- 2026-11-05: wl-gammarelay-rs requires wlr-gamma-control-unstable-v1: one owner per output, ramp size announced by the compositor, original tables restored on destroy. Exposes per-output Temperature, Gamma, Brightness and Inverted properties on the session bus. Declared GPL-3.0-only (wl-gammarelay-rs@9874e4a) [source] <https://github.com/MaxVerevkin/wl-gammarelay-rs/blob/9874e4a273a01f60e578a896183d67aa7a23572c/src/wayland.rs#L36> <https://gitlab.freedesktop.org/wlroots/wlr-protocols/-/blob/1106a4a1c57562ddd438c547da4c0d6dc150d624/unstable/wlr-gamma-control-unstable-v1.xml>
- 2026-11-09: The SDK color-temperature call persists across shell reloads and targets one or all displays. It leaves nested desktops and monitors controlled by another gamma client unchanged [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/system-calls.ts#L156>
- 2026-11-05: wl-gammarelay-rs keeps temperature (clamped 1000 to 10000 K), gamma as a direct x^gamma exponent, brightness and inversion per output. A root setter averages across outputs, and a newly attached output inherits that aggregate rather than remembered per-monitor values (wl-gammarelay-rs@9874e4a) [source] <https://github.com/MaxVerevkin/wl-gammarelay-rs/blob/9874e4a273a01f60e578a896183d67aa7a23572c/src/color.rs#L1>
- 2026-11-08: The SDK method setColorTemperature(kelvin, displayName) applies a per-display night-light gamma tint from 1000 to 6500 K, where 6500 K leaves colors unchanged. It is a gamma adjustment, not ICC calibration [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/displays.ts#L36>
- 2026-11-09: The display docs set night light as a color temperature for every monitor or for one by display name, 1000 to 6500 with 6500 neutral; a nested desktop does nothing because the host compositor owns the screen [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L270>
- 2026-11-04: Before [#1915](https://github.com/cprussin/domicile/issues/1915), the engine exposed one global white point and profiles carried no per-panel temperature [docs]
- 2026-11-09: Viz’s candidate factory rejects nonidentity output color matrices. Ash’s hardware-CTM and compositor-matrix branches therefore have distinct source contracts; actual output promotion still requires separate evidence (ash-and-viz@domicile-engine-pin) [inferred] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/viz/service/display/overlay_candidate_factory.cc#L142> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/viz/service/display/overlay_candidate_factory.cc#L226> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/system/night_light/night_light_controller_impl.cc#L147>

## Where it touches Domicile

- `compositor` arbitrates ramp ownership and chooses client or night-light state. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L276) [docs]
- `engine:drm` sends each ramp through the display delegate and reapplies it after a modeset. Display reports resend ramps after hotplug or engine restart. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L284) [docs]
- Status at the pin: landed-with-gap. [#1921](https://github.com/cprussin/domicile/pull/1921) answers the per-output ramp behavior raised in [#1915](https://github.com/cprussin/domicile/issues/1915). [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L266) [docs]

- A new night-light value can wait for another rendered frame before becoming visible on an idle display. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md#L41) [docs]

## Lineage

- 2026-11-09: the pinned display contract records per-output ramp ownership, restoration and hardware limits. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L266) [docs]

## Sources

- S-8100b35ed50a · 2026-11-09 · Pinned upstream behavior and client limits.

- S-cf898850da22 · 2026-11-09 · Night-light all-output caller.

- S-7cd85cb7ef61 · 2026-11-09 · Named temperature and client-ramp ownership.

- S-43af2b3af183 · 2026-11-09 · Pinned donor implementation locator.

- S-e7392f226f69 · 2026-11-09 · Pinned donor implementation locator.

- S-45b621140a35 · 2026-11-09 · Night-light override and schedule lifetime.

- S-74f557738f56 · 2026-11-09 · Output profiles, pointer transforms and gamma contract.
