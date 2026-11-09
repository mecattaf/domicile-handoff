---
id: protocols/collections/zcr
title: zcr protocol collection
kind: collection
family: protocols
layers: [compositor, protocol, external]
domicile_status: n/a
domicile_refs: []
checked_at: "2026-11-09"
scroll_parity: baseline
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [layer:protocol, evidence:source, scroll-parity:baseline]
sources: [S-000764dce113, S-04faf44f2e81, S-06ca08859f6e, S-135ecf2b8dea, S-18ec6dfe35f6, S-1b81a86927e0, S-1d7075319a3c, S-1edc2e41b6da, S-2f9c9a88b407, S-3138604e8df0, S-37cd947718b7, S-3acbfb4907e7, S-3f4edde82525, S-3f524db52413, S-51292cdb8ce9, S-5fcf9ba138c3, S-610c446f4416, S-67ccf82fd285, S-6fceeb23f0e9, S-8c6da920d0f9, S-9297edaee809, S-961f39214831, S-a19d3732cf8f, S-a542cf9c1edb, S-a89a584c0175, S-bad77f83f7f6, S-c5dec4346034, S-c977034319b9, S-d6eb10fae754, S-da09135fa657, S-dc8001ed8842, S-de5e3879f7ce, S-df27d92d57a0, S-eb54787f3a5f]
---

# zcr protocol collection

ChromeOS and Exo vendor protocols, including Aura wire surfaces. Collection names group this map; a collection label does not change the specification’s lifecycle or maturity. A dated family or module alias does not establish an exact wire version.

## At the pin

| Protocol | Purpose | Domicile | Evidence and limit |
|---|---|---|---|
| `overlay_prioritizer` | Attach hardware-overlay hints to a surface | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/components/exo/wayland/protocol/overlay-prioritizer.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `surface_augmenter` | Attach clipping, corner and subsurface rendering information | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/components/exo/wayland/protocol/surface-augmenter.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zaura_output_manager` | Push ChromeOS output state to clients | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/components/exo/wayland/protocol/aura-shell.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zaura_output_manager_v2` | Push ChromeOS output state through the revised read-only contract | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/components/exo/wayland/protocol/aura-output-management.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zaura_shell` | Attach ChromeOS window, surface and output state to native clients | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/components/exo/wayland/protocol/aura-shell.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zcr_alpha_compositing_v1` | Attach blending and opacity information to surfaces | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/alpha-compositing/alpha-compositing-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zcr_cursor_shapes_v1` | Select a Chromium vendor cursor shape | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/cursor-shapes/cursor-shapes-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; the separate standard cursor-shape manager is advertised [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1302) |
| `zcr_extended_drag_v1` | Coordinate enhanced drag behavior such as tab detachment | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/extended-drag/extended-drag-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zcr_gaming_input_v2` | Expose gaming device information and input | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/gaming-input/gaming-input-unstable-v2.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zcr_keyboard_configuration_v1` | Describe keyboard configuration to a client | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/keyboard/keyboard-configuration-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zcr_keyboard_extension_v1` | Acknowledge whether client key events were handled | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/keyboard/keyboard-%65xtension-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zcr_notification_shell_v1` | Create and track vendor notification surfaces | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/notification-shell/notification-shell-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zcr_remote_shell_v1` | Exchange legacy ChromeOS remote window state | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/remote-shell/remote-shell-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zcr_remote_shell_v2` | Exchange revised ChromeOS remote window state | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/remote-shell/remote-shell-unstable-v2.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zcr_secure_output_v1` | Mark surface contents for screenshot exclusion and secure-output display | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/secure-output/secure-output-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zcr_stylus_tools_v1` | Control stylus-only input behavior for a surface | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/stylus-tools/stylus-tools-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zcr_stylus_v2` | Report stylus pressure, tilt and tool information | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/stylus/stylus-unstable-v2.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zcr_text_input_extension_v1` | Add richer operations to text-input-v1 | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/text-input/text-input-%65xtension-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; the vendor object attaches to zwp_text_input_v1; this is not a text-input-v3 object [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/text-input/text-input-%65xtension-unstable-v1.xml#L55) |
| `zcr_touchpad_haptics_v1` | Request touchpad haptic feedback | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/touchpad-haptics/touchpad-haptics-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zcr_ui_controls` | Provide a build-specific input simulation contract | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/ui-controls/ui-controls-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `zcr_vsync_feedback_v1` | Report compositor timing feedback | absent; not advertised; client cannot bind a global for this wire contract at the pin | vendor schema purpose [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/vsync-feedback/vsync-feedback-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |

## Lineage

| Protocol | Date | Documentary finding | Source |
|---|---|---|---|
| `overlay_prioritizer` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `surface_augmenter` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zaura_output_manager` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zaura_output_manager` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zaura_output_manager_v2` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zaura_output_manager_v2` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zaura_shell` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zaura_shell` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zaura_shell` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `zcr_alpha_compositing_v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_alpha_compositing_v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zcr_cursor_shapes_v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_cursor_shapes_v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zcr_cursor_shapes_v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `zcr_extended_drag_v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_extended_drag_v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zcr_gaming_input_v2` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_gaming_input_v2` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zcr_keyboard_configuration_v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_keyboard_configuration_v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zcr_keyboard_extension_v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_keyboard_extension_v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zcr_notification_shell_v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_notification_shell_v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zcr_remote_shell_v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_remote_shell_v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zcr_remote_shell_v1` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `zcr_remote_shell_v2` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_remote_shell_v2` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zcr_remote_shell_v2` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `zcr_secure_output_v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_secure_output_v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zcr_secure_output_v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `zcr_secure_output_v1` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `zcr_stylus_tools_v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_stylus_tools_v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zcr_stylus_v2` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_stylus_v2` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zcr_text_input_extension_v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_touchpad_haptics_v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_touchpad_haptics_v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zcr_ui_controls` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_ui_controls` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `zcr_vsync_feedback_v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `zcr_vsync_feedback_v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |

## Sources

- [Complete coverage map](../coverage-map.md).
- S-000764dce113 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-04faf44f2e81 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-06ca08859f6e · 2026-11-09 · Pinned Chromium 150 vendor XML contract.
- S-135ecf2b8dea · 2026-11-09 · Pinned Chromium 150 vendor XML contract.
- S-18ec6dfe35f6 · 2026-11-09 · Protocol implementation and registration evidence.
- S-1b81a86927e0 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-1d7075319a3c · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-1edc2e41b6da · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-2f9c9a88b407 · 2026-11-09 · Pinned Chromium 150 vendor XML contract.
- S-3138604e8df0 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-37cd947718b7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-3acbfb4907e7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-3f4edde82525 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-51292cdb8ce9 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-5fcf9ba138c3 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-610c446f4416 · 2026-11-09 · Pinned Chromium 150 vendor XML contract.
- S-67ccf82fd285 · 2026-07-16 · Historical protocol appendix.
- S-6fceeb23f0e9 · 2026-11-09 · Pinned Chromium 150 vendor XML contract.
- S-8c6da920d0f9 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-9297edaee809 · 2026-11-09 · Protocol implementation and registration evidence.
- S-961f39214831 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-a19d3732cf8f · 2026-06-11 · Historical protocol audit.
- S-a542cf9c1edb · 2026-07-16 · Protocol implementation and registration evidence.
- S-a89a584c0175 · 2026-11-09 · Protocol implementation and registration evidence.
- S-bad77f83f7f6 · 2026-11-09 · Protocol implementation and registration evidence.
- S-c5dec4346034 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-c977034319b9 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-d6eb10fae754 · 2026-11-09 · Protocol implementation and registration evidence.
- S-da09135fa657 · 2026-11-09 · Protocol implementation and registration evidence.
- S-dc8001ed8842 · 2026-11-09 · Pinned Chromium 150 vendor XML contract.
- S-de5e3879f7ce · 2026-07-16 · Protocol implementation and registration evidence.
- S-df27d92d57a0 · 2026-11-09 · Pinned Chromium 150 vendor XML contract.
- S-eb54787f3a5f · 2026-11-09 · Protocol implementation and registration evidence.
