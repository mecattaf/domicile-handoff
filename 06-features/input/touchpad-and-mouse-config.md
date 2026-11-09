---
id: input/touchpad-and-mouse-config
title: Configure touchpads and mice
kind: feature
family: input
layers: [engine:input, config, session]
domicile_status: landed
domicile_refs: [cprussin/domicile#1877, cprussin/domicile#1879, cprussin/domicile#1884]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: true
scroll_parity: baseline
donors: [sway, scroll, hyprland, mutter, miracle-wm]
rows: []
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:input, status:landed, evidence:source, scroll-parity:baseline]
sources: [S-069533fa6b43, S-09fd0ca98c14, S-1951a846e891, S-1be0caf83f39, S-2c05e7f10be8, S-2d0a5d452253, S-44ee8a5b496a, S-46b17bd8741b, S-4819d73df19e, S-4a11b3f74e93, S-4e3e5cda1048, S-500fe6fd17da, S-5a321c8ce8ea, S-5ac26707cdda, S-6d51bf619041, S-800e53f29a05, S-943fa4269d7f, S-99698015bc48, S-9af7e3f180f1, S-a5c38d248e25, S-a89a4c46b8f9, S-a96fac7a32a5, S-b056064c5b44, S-baadedc7c85c, S-c92aa706df61, S-d2ff8f48a9b8, S-d514f4fb223b, S-d8ec1ebf2aab, S-e218e6d49ccf, S-e21a73fec98d, S-e92a22a88dc7, S-ef437e2b7375, S-fcd733bf5e55]
---
# Configure touchpads and mice

Touchpad and mouse settings reach the tty engine through a validated configuration snapshot. Native libinput pairing and contact suppression remain available alongside explicit typing policies. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/input_settings.cc#L106) [source]

## Behavior

- Touchpad settings include tap mapping, tap-drag, drag lock, natural scrolling, acceleration profile/speed, click and scroll method, middle emulation, scroll factor and typing suppression. Mouse settings expose the applicable pointer and wheel controls. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/input_settings.cc#L106) [source]
- [Typing suppression](disable-while-typing.md) covers native libinput delegation, the custom guard and contact-lifetime limits.
- Unsupported explicit click/scroll methods return an error. Omitted values resolve to declared defaults when parsing the snapshot. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/input_settings.h#L48) [source]


- Tap mapping accepts LRM or LMR. The schema separately documents physical clickfinger as fixed LRM, with no independent selector for that mapping. [source] [Tap and click configuration](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/input_devices.rs#L26) · [Physical clickfinger boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/input_devices.rs#L140)

- Absolute-pointer mappings select a device by exact name or physical path and name one display target. Their schema exposes no crop, calibration, tablet-tool-mode or whole-desktop target field. [source] [Mapping fields](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/pointers.rs#L12)

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | libinput configuration and seat-level typing behavior | Native pairing distinguishes an internal keyboard from unrelated external input | [source](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/libinput.c#L273) |
| scroll | Carries sway input semantics alongside layout gestures | Native configuration remains separate from layout motion | [source](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/libinput.c#L273) |
| Hyprland @ source pin | Native libinput typing suppression is conditional on device support | The DWT setter and acceleration clamp are independent controls | [Input configuration](https://github.com/hyprwm/Hyprland/blob/ab6141897d57e609e24c571a762c30c963e0885a/src/managers/input/InputManager.cpp#L1461) |
| GNOME Mutter | Capability-checked native libinput DWT and timeout setter | The timeout is forwarded to libinput in milliseconds | [source](https://github.com/GNOME/mutter/blob/b766717ac9ad04d44211557ec79a545dc8c44ee7/src/backends/native/meta-input-settings-native.c#L259) · [source](https://github.com/GNOME/mutter/blob/b766717ac9ad04d44211557ec79a545dc8c44ee7/src/backends/native/meta-input-settings-native.c#L278) |
| Miracle WM @ 0.11.2 | Acceleration, per-axis scroll speeds and touchpad typing suppression are separate settings | Its docs default mouse acceleration to none and typing suppression to false; those fields establish no gesture binding | [Mouse settings](https://github.com/miracle-wm-org/miracle-wm/blob/v0.11.2/wiki/docs/configuration/input/mouse.md#L15) · [Touchpad settings](https://github.com/miracle-wm-org/miracle-wm/blob/v0.11.2/wiki/docs/configuration/input/touchpad.md#L16) [docs] |

## What the notes found

- The native-input review separates libinput-owned contact suppression from custom timer policies. A timeout does not prove that a held contact becomes active. S-4e3e5cda1048 · 2026-11-06 · Physical input qualification plan [docs]
- The preserved qualification plan distinguishes nested input, which belongs to the host, from tty acquisition. The upstream validation scope does not establish physical pointer behavior. [Validation boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/A-DESKTOP-ON-A-TTY.md#L53) [docs]
- 2026-11-09: The selected libinput mouse converter applies acceleration profile and speed through native setters when acceleration is available. Config changes schedule device-setting updates; source establishes application beyond parser storage, without physical-mouse qualification [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0173-domicile-configure-touchpads-and-mice-during-a-session.patch#L40> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0173-domicile-configure-touchpads-and-mice-during-a-session.patch#L231> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0059-domicile-a-mouse-is-accelerated-and-its-wheel-scroll.patch#L65>
- 2026-11-09: The later shared-seat patch routes mice and pointing sticks, including combined keyboard devices, through the libinput mouse feature. Device-setting application retains their mouse-specific settings path after that refactor [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0175-domicile-share-libinput-devices-and-native-typing.patch#L149> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0175-domicile-share-libinput-devices-and-native-typing.patch#L848>
- 2026-11-09: Tap button mapping selects LRM or LMR. Physical clickfinger has a documented fixed LRM mapping, with no separate mapping selector in the complete touchpad schema [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/input_devices.rs#L26> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/input_devices.rs#L140>
- 2026-11-09: At the pin the config crate has an input_devices module with a natural_scroll setting defaulting off, and engine patch 'configure touchpads and mice during a session' is in the series [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/input_devices.rs#L21> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0173-domicile-configure-touchpads-and-mice-during-a-session.patch>
- 2026-11-04: The scroll input manual resolves settings by identifier, then `type:<input_type>`, then wildcard; a later type or wildcard command overrides earlier identifier settings. xkb_capslock and xkb_numlock apply in the configuration file only (scroll@b6586d2) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-input.5.scd#L13> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-input.5.scd#L91>
- 2026-11-09: InputConfig groups keyboard, touchpad, mouse and absolute-pointer mappings. Its configuration scope differs from sway’s per-device selector blocks [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/lib.rs#L227>
- 2026-11-05: sway-input documents click_method, drag_lock (disabled, enabled, enabled_sticky), scroll_method (none, two_finger, edge, on_button_down) and natural_scroll as per-device settings; type and wildcard selectors can override an identifier when applied by command. [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-input.5.scd#L159-L211>
- 2026-11-05: On a 2026-11-05 main snapshot the input config held keyboard settings and absolute-pointer display bindings only; natural scroll, tap, click method and acceleration profile had no config key [source]
- 2026-11-06: Inverted two-finger scrolling on a touchpad was a libinput natural-scroll setting; enabling it through the engine flag on a matched source and engine pair corrected the direction on 2026-11-06 [measured] <https://github.com/cprussin/domicile/pull/1884>
- 2026-11-07: Input PR 1884 merged 2026-11-06; on 2026-11-07 a Magic Trackpad disconnect and reconnect via BlueZ released and reacquired the event device without an engine restart. [measured] <https://github.com/cprussin/domicile/pull/1884>
- 2026-11-05: Native libinput disable-while-typing requires keyboards and touchpads on one libinput seat; at the earlier snapshot each pad had its own context. A shared-seat backend kept keyboard filters, layout and repeat, and a configurable guard with adjustable pauses stayed available [measured] <https://wayland.freedesktop.org/libinput/doc/1.26.0/palm-detection.html>
- 2026-11-04: In a disposable KVM guest with synthetic devices, enabling libinput natural scrolling on a touchpad flipped its scroll sign while a wheel mouse stayed unchanged; recreating the devices restored defaults until the setting was applied again (libinput) [measured]
- 2026-11-04: libinput's flat pointer profile gave identical displacement for identical raw motion at two event rates in the guest, while the default adaptive profile moved farther at the faster rate (libinput) [measured]
- 2026-11-05: A native-controller patch with natural scroll and flat mouse acceleration as opt-in flags, built on the 2026-11-05 main snapshot, kept natural scroll across touchpad re-creation in a DRM guest, left wheel direction unchanged, and made flat motion independent of event rate [measured]
- 2026-11-06: On a build carrying touchpad settings, two-finger scrolling and two-finger back/forward navigation in an external Chrome window worked on a Magic Trackpad on 2026-11-06 [measured]

## Where it touches Domicile

- `engine:input` parses finite speed and factor values, choices and typing policy. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/input_settings.cc#L56) [source]
- `engine:input` places authorized keyboard and pointer descriptors in a shared libinput context; callbacks duplicate only descriptors already granted by logind. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/libinput_seat.cc#L37) [source]
- Status at the pin: landed. The early configuration requests [#1877](https://github.com/cprussin/domicile/pull/1877) and [#1879](https://github.com/cprussin/domicile/pull/1879) are answered by [#1884](https://github.com/cprussin/domicile/pull/1884). The pinned settings parser and defaults provide the checkable contract. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/input_settings.h#L22) [source]

- The engine patches “domicile: configure touchpads and mice during a session” and “domicile: share libinput devices and native typing”, introduced by [#1884](https://github.com/cprussin/domicile/pull/1884), route mice and pointing sticks through the native settings path. That path calls acceleration profile and speed setters when supported, beyond storing parsed settings. [source] [Native setters](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0173-domicile-configure-touchpads-and-mice-during-a-session.patch#L231) · [Shared-seat routing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0175-domicile-share-libinput-devices-and-native-typing.patch#L149)

## Lineage

- 2026-11-06: the input changes consolidated settings validation, source/stop metadata and shared input ownership. S-800e53f29a05 · 2026-11-06 · Native input source audit [docs]
- 2026-11-09: settings and shared authorized input context are present at the pinned revision. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/input_settings.cc#L157) [source]

## Sources

- S-a96fac7a32a5 · 2026-11-09 · Miracle WM touchpad configuration contract.

- S-d8ec1ebf2aab · 2026-11-09 · Miracle WM mouse configuration contract.

- S-e92a22a88dc7 · 2026-11-09 · Shared input-seat routing.

- S-baadedc7c85c · 2026-11-09 · Native mouse settings application.

- S-5a321c8ce8ea · 2026-11-09 · Absolute-pointer mapping fields.

- S-ef437e2b7375 · 2026-11-09 · Tap and physical clickfinger configuration scope.

- S-1951a846e891 · 2026-11-09 · Direct-session architecture and input validation scope.

- S-c92aa706df61 · 2026-11-09 · Mutter native input settings.

- S-2d0a5d452253 · 2026-11-09 · Pinned donor implementation locator.

- S-1be0caf83f39 · 2026-11-09 · Pinned donor implementation locator.

- S-e218e6d49ccf · 2026-11-09 · Hyprland input configuration and device lifetime.

- S-4819d73df19e · 2026-11-09 · Typing suppression and device button lifetime.

- S-44ee8a5b496a · 2026-11-06 · Native input contribution scope and qualification limits.
- S-4e3e5cda1048 · 2026-11-06 · Physical input qualification plan.
- S-500fe6fd17da · 2026-11-09 · Engine input-setting validation and policy choices.
- S-800e53f29a05 · 2026-11-06 · Native input source audit.
- S-99698015bc48 · 2026-11-09 · Engine input defaults and snapshot contract.
- S-e21a73fec98d · 2026-11-09 · Shared libinput context with authorized device descriptors.
