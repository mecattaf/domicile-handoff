---
id: aura-wm/display-management
title: Display topology and configuration
kind: feature
family: aura-wm
layers: [engine:drm, compositor, config]
domicile_status: unknown
checked_at: "2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc"
answered_upstream: false
scroll_parity: n/a
donor_availability: chromeos-gated
donors: [ash]
evidence_mix: mixed
boundary_note: false
bucket: domicile
tags: [family:aura-wm, donor:ash, evidence:source, bucket:domicile]
sources: [S-09fd0ca98c14, S-33889728083c, S-3533e3f0274a, S-578137f975a9, S-5a9e3b32e108, S-5d00de850efd, S-6c3bd66774f8, S-6cf2e26c2a42, S-704b41b30c0d, S-74f557738f56, S-9d1497c20da4, S-af414fdb64d0, S-f4692b1b46b5]
eras: [Aura lift (July 2026), views-shell]
rows: [AL-display-mgmt-001, AL-display-mgmt-002, AL-display-mgmt-003, AL-display-mgmt-005, AL-display-mgmt-006, AL-display-mgmt-007, AL-display-mgmt-008, AL-display-mgmt-009, AL-display-mgmt-010, AL-display-mgmt-013, AL-display-mgmt-014, AL-display-mgmt-015, AL-display-mgmt-016, AL-cade-machinery-wm-032, AL-cade-machinery-wm-033, AL-exo-target-dependencies-006, AL-ash-window-state-inputs-001, AL-engine-check-011, AL-engine-check-015]
depends_on: []
reported_on_domicile: []
domicile_refs: [docs/DISPLAYS.md]
---
# Display topology and configuration

Display configuration joins connector identity, modes, scale, rotation and relative placement. A stored arrangement is separate from the platform operation that applies it. [source] ([Layout store](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/display/manager/display_layout_store.h#L39))

## Behavior

- Ash initializes the primary root separately; secondary roots notify Shell observers only after layout, wallpaper and desk initialization. The primary branch does not emit that root-added notification. [source] ([Donor implementation 1](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/root_window_controller.cc#L1009); [Donor implementation 2](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/shell_observer.h#L23)).

- Stored topology, output capabilities and applied configuration are separate pieces of state. [inferred]
- Physical dimensions, logical scale and refresh rate are distinct values in the donor. [source] ([Mode metadata](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/display/manager/managed_display_info.h#L60))

## How donors do it

| Donor @ pin | Mechanism | Notable behavior or limit | Link |
|---|---|---|---|
| Chromium display manager @ engine pin | `DisplayLayoutStore` keys layouts by a list of display identities | Default placement, forced mirror state and unified-desktop preference are separate fields | [Layout store](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/display/manager/display_layout_store.h#L39) |
| Chromium display manager @ engine pin | `ManagedDisplayInfo` carries mode and output metadata | Refresh, physical size, scale and identity are not interchangeable | [Display metadata](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/display/manager/managed_display_info.h#L60) |

- Ash: Ash display shutdown destroys its rotation animator before optional window migration, blocks further event targeting, closes child windows and invalidates display lookup. [source] ([Lifecycle source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/root_window_controller.cc#L702)).


- ChromeOS-gated Ash display migration scales normal or minimized window origins relative to destination size, fits them to its work area, adjusts restore bounds separately and rebuilds stacking from the all-desks MRU order. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/root_window_controller.cc#L180), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/root_window_controller.cc#L271).

## What the notes found

- A 2026-10-06 study separated KMS machinery from seat acquisition and VT handling. Its architectural estimates do not establish a current Domicile gap. [docs] S-f4692b1b46b5.
- 2026-11-08: Display profile placement carries a fractional f64 scale. Logical size is the pixel size over the scale, rounded to nearest: 5120 by 2880 at 1.8 gives 2844 by 1600, and 1.8 is representable as 216/120 in fractional-scale units [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/profile.rs#L141>
- 2026-11-05: Output profiles match by `drm-<id>` names rather than `eDP-n` when EDID exists, identical panels share a description, mode only validates dimensions without selecting refresh rate, and adaptive sync is unsupported [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md>
- 2026-11-08: Display profiles match by the exact set of connected displays, so a laptop with two panels needs separate profiles for the external monitor plus both panels and for the monitor plus one panel [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md>
- 2026-11-05: The pinned [display documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md) covers per-display rotation in the render tree, touchscreen delivery to one bound window, monitor naming compatible with sway and kanshi, and night light as a gamma ramp. [docs]

## Where it touches Domicile

- The pinned [display documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md) is the current contract. The donor target is `chromeos-gated`; its algorithms can still inform a separate rule port. [inferred]
- Output configuration crosses engine:drm, compositor delivery and persistent configuration. Data structures alone do not decide which layer owns a user-facing policy. [inferred]

- At the pinned checkout, rotation happens in the render tree, profiles retain output identity across hotplug, and mode assertions do not select a mode or refresh rate. [docs] [Display contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L28).

- The display-manager target explicitly asserts ChromeOS. [source] [Build gate](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/display/manager/BUILD.gn#L7).

## Lineage

- 2026-07-18: the Aura study separated display rules from ChromeOS coupling. [docs]
- 2026-10-06: the survey revisited DRM, display metadata and seat integration separately. [docs]

## Sources

- S-704b41b30c0d · 2026-11-09 · Chromium display layout storage interface
- S-578137f975a9 · 2026-11-09 · Chromium display metadata interface
- S-f4692b1b46b5 · 2026-10-06 · Aura compositor and window management survey
- S-5d00de850efd · 2026-11-09 · Pinned display-manager target gate
- S-af414fdb64d0 · 2026-11-09 · Ash primary and secondary display initialization
- S-6cf2e26c2a42 · 2026-11-09 · Ash shell observer lifecycle contract
- S-74f557738f56 · 2026-11-09 pin check · Pinned display topology and presentation contract
