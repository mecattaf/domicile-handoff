---
id: "02-aura-ash-exo/display-mgmt"
title: "Display management"
kind: "collection"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile", "evidence:source"]
sources: [S-1f554a1713a5, S-578137f975a9, S-6e92b29ab369, S-6f7abf0045c1, S-704b41b30c0d, S-b59590b62aaa, S-ee55709cced8, S-74f557738f56]
---

# Display management

Chromium separates display identity, stored topology and platform configuration. ChromeOS manager classes and Linux DRM integration have different availability. [docs]

## What Chromium already has

The [row data](rows/aura-lift.tsv) holds 15 rows for this beat: ADAPT 8, FRESH 1, LIFT 6. These are historical source verdicts. [docs]

Donor availability in those rows: `chromeos-gated` 9, `in-engine-build` 6. Zero Ash references in a file do not bypass its target build gate. [docs]

- DisplayLayoutStore registers a layout against a set of connected display identifiers, a structural analog of display-combination profiles. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/display/manager/display_layout_store.h#L42).
- ManagedDisplayInfo stores zoom values per display size and combines zoom with device scale; this is distinct from proving a particular fractional-scale wire implementation. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/display/manager/managed_display_info.h#L194), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/display/manager/managed_display_info.h#L256).
- HeadlessScreenManager delegates logical display creation and removal without requiring the delegate to expose a DRM modesetting API. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/display/headless/headless_screen_manager.h#L27).
- Ash NightLightController exposes a color-temperature setter and depends on Ash display and session controllers; API similarity does not establish Linux build availability. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/system/night_light/night_light_controller_impl.h#L111), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/system/night_light/night_light_controller_impl.cc#L327).
- Ash night light [tries a hardware color matrix before compositor fallback](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/system/night_light/night_light_controller_impl.cc#L147), applies the transform per display host, and skips the unified virtual host to avoid duplicate application. This ChromeOS-gated execution path is more specific than its temperature-setting interface. [source]

## Domicile at the pin

- `engine:drm, compositor and config`: The pinned display contract distinguishes profile assertions from selecting modes, preserves output identity across hotplug, and rotates through the render tree. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L28).
- `engine:drm and compositor`: Night light and gamma-control ownership are per output, with a protocol client temporarily owning its monitor ramp. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L266).
- [Night light #1921](https://github.com/cprussin/domicile/pull/1921) records the Domicile per-output gamma mechanism. Similar naming does not establish an identical Ash mechanism. [docs]

## Boundary

Display data structures do not establish seat, modeset or output-policy ownership. The current contract places configuration in Rust and DRM application in the engine. [inferred]

## Feature links

- [aura-wm/display-management](../06-features/aura-wm/display-management.md)

## Sources

- S-6e92b29ab369 · 2026-07-18 · Chromium display management source and equivalence inventory
- S-1f554a1713a5 · 2026-07-18 · Lift-map evaluation and corrections
- S-578137f975a9 · 2026-11-09 · Pinned donor interface inspection
- S-6f7abf0045c1 · 2026-11-09 · Pinned donor interface inspection
- S-704b41b30c0d · 2026-11-09 · Pinned donor interface inspection
- S-b59590b62aaa · 2026-11-09 · Pinned donor interface inspection
- S-ee55709cced8 · 2026-11-09 · Pinned donor interface inspection
- S-74f557738f56 · 2026-11-09 pin check · Pinned display topology and presentation contract
