---
id: protocols/collections/staging
title: staging protocol collection
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
sources: [S-016ff15271ae, S-0aa2cc30bb07, S-0b6c31dfe6bc, S-0d6c45f5a62b, S-1389e89c68c6, S-18a2678e3352, S-1c210691c0b8, S-2b83da449b94, S-2ea0540e0f1d, S-32784ffd9a8d, S-33085e8f5a27, S-385433e1522c, S-3acbfb4907e7, S-3c995b7a34f5, S-3d63fd27af2d, S-3e03b7086707, S-3e37be57c91a, S-3f524db52413, S-4378aebc583f, S-43af2b3af183, S-44598db24690, S-44be82b2b78e, S-46587779628d, S-46f8f6ded930, S-4aa1e405fd79, S-4cd685c56e61, S-4d83eed81e29, S-534dcadd1369, S-548e570973fa, S-5e4ce5d63275, S-5f315b39ac7c, S-66b2ff508daa, S-67ccf82fd285, S-703c0d408b29, S-70da4c3a822b, S-7170fa893ec4, S-728ccd41be9e, S-7b31c5bb3be0, S-7f8302736ae8, S-7fff5f5a9d7c, S-82b445bd2404, S-866b9f3f829d, S-88f5036dac2f, S-9081893da4f7, S-92d9911f0704, S-9a9f91803d7e, S-a04ed2a57dc2, S-a19d3732cf8f, S-a3671b8e50eb, S-a542cf9c1edb, S-a90d2490bf23, S-b2415d10a041, S-b4058c3be3bd, S-b61833f7b205, S-b824688e0e5c, S-bad77f83f7f6, S-c2596d00b100, S-ca6aadf58810, S-ccf976691aa5, S-ce8dab4e8148, S-d2752f2d6c22, S-d8f8e61b831b, S-de5e3879f7ce, S-e2a9bd2f0d14, S-e4ff18df1f16, S-e7171507c7aa, S-e7392f226f69, S-e8cd22f8b451, S-e9dc5003d109, S-ef628d581794, S-f41f214e88e8, S-f5d75cc3208b, S-f79b692d55fc, S-fcc1e24ec465, S-fcc50bf0b486, S-fd840029644f, S-ff3fcf31d13d, S-ff8255d1567e]
---

# staging protocol collection

Staging, legacy unstable and custom schemas, with lifecycle recorded separately. Collection names group this map; a collection label does not change the specification’s lifecycle or maturity. A dated family or module alias does not establish an exact wire version.

## At the pin

| Protocol | Purpose | Domicile | Evidence and limit |
|---|---|---|---|
| `alpha-modifier-v1` | Apply a multiplier to surface opacity | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/alpha-modifier/alpha-modifier-v1.xml) ; schema lifecycle staging ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/alpha_modifier/mod.rs#L169) |
| `color-management-v1` | Describe output and surface color characteristics through image descriptions and rendering intent | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/color-management/color-management-v1.xml) ; schema lifecycle staging ; image-description purpose [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/color-management/color-management-v1.xml#L31) ; Scroll optional ICC creator reports unsupported feature [source] [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/types/wlr_color_management_v1.c#L856) ; Scroll color-manager optional feature flags are narrower than the XML schema [source] [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/types/wlr_color_management_v1.c#L986) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `color-representation-v1` | Describe alpha mode, YCbCr conversion, subsampling and quantization range | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/color-representation/color-representation-v1.xml) ; schema lifecycle staging ; representation metadata excludes resulting RGB colorimetry [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/color-representation/color-representation-v1.xml#L29) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `commit-timing-v1` | Set a presentation-time constraint on a surface commit | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/commit-timing/commit-timing-v1.xml#L27) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/commit_timing/mod.rs#L141) |
| `content-type-v1` | Attach a content-type rendering hint to a surface | unknown; advertised, 1 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/content-type/content-type-v1.xml) ; schema lifecycle staging ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L801) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/content_type/mod.rs#L166) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `cursor-shape-v1` | Request a cursor using a standard shape name | unknown; advertised, 2; named cursor maps to engine CSS cursor; surface image falls back to arrow | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/cursor-shape/cursor-shape-v1.xml) ; schema lifecycle staging ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1302) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/cursor_shape.rs#L137) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `domicile-anchor-v1` | Place a native window at a named anchor in a shell page | landed; advertised, 1; Domicile anchored native windows | additional schema declarations [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/protocols/domicile-anchor-v1.xml#L40) ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/anchors.rs#L35) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1436) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L112) |
| `drm-lease-v1` | Lease non-desktop DRM connectors to a client | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/drm-lease/drm-lease-v1.xml) ; schema lifecycle staging ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available; backend_drm feature-gated, not selected by compositor package manifest [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/drm_lease/mod.rs#L429) ; compositor package features [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/Cargo.toml#L84) |
| `fifo-v1` | Coordinate surface commits with refresh-paced ordering | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/fifo/fifo-v1.xml#L26) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/fifo/mod.rs#L137) |
| `fractional-scale-v1` | Report a fractional preferred scale to a surface | landed; advertised, 1 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/fractional-scale/fractional-scale-v1.xml) ; schema lifecycle staging ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L799) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/fractional_scale/mod.rs#L92) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/933) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md#L124) |
| `fullscreen-shell-v1` | Present a surface using the older dedicated fullscreen-shell role | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/fullscreen-shell/fullscreen-shell-unstable-v1.xml#L29) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; normal xdg-toplevel fullscreen requests have a separate handler [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/xdg_shell.rs#L276) |
| `idle-inhibit-v1` | Inhibit automatic idle actions while a surface is active | landed; advertised, 1; always advertised even when idle timer unset | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/idle-inhibit/idle-inhibit-unstable-v1.xml) ; schema lifecycle unstable ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L804) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/idle_inhibit/mod.rs#L64) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1608) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/IDLE.md#L85) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/IDLE.md#L92) |
| `input-method-v2` | Connect an input-method client to text composition and popup delivery | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/protocol/input-method-unstable-v2.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/input_method/mod.rs#L154) |
| `input-timestamps-v1` | Attach higher precision timestamps to input events | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/input-timestamps/input-timestamps-unstable-v1.xml#L41) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `keyboard-shortcuts-inhibit-v1` | Let a focused client inhibit compositor shortcuts | landed; advertised, 1 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/keyboard-shortcuts-inhibit/keyboard-shortcuts-inhibit-unstable-v1.xml) ; schema lifecycle unstable ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1280) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/keyboard_shortcuts_inhibit/mod.rs#L76) ; status covers the shortcut-inhibitor capability; shell-grabbed gesture delivery has a documented limit [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L144) ; merged PR known gap [source] [source](https://github.com/cprussin/domicile/pull/1913) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1343) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L144) |
| `linux-drm-syncobj-v1` | Synchronize buffer use with explicit DRM timelines | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/linux-drm-syncobj/linux-drm-syncobj-v1.xml) ; schema lifecycle staging ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available; backend_drm feature-gated, not selected by compositor package manifest [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/drm_syncobj/mod.rs#L149) ; compositor package features [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/Cargo.toml#L84) |
| `linux-explicit-synchronization-v1` | Synchronize surface buffer use with acquire and release sync files | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/linux-explicit-synchronization/linux-explicit-synchronization-unstable-v1.xml#L29) ; the XML marks this sync-file contract as superseded by linux-drm-syncobj ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; exact pinned schema-target name [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/BUILD.gn#L90) |
| `pointer-constraints-v1` | Lock or confine a pointer to a surface region | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/pointer-constraints/pointer-constraints-unstable-v1.xml) ; schema lifecycle unstable ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/pointer_constraints.rs#L220) |
| `pointer-gestures-v1` | Deliver pinch, swipe and hold gesture sequences | landed; advertised, 3; pinch/swipe/hold | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/pointer-gestures/pointer-gestures-unstable-v1.xml) ; schema lifecycle unstable ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L869) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/pointer_gestures.rs#L390) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1911) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1928) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L25) |
| `pointer-warp-v1` | Request a compositor-mediated pointer position change | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/pointer-warp/pointer-warp-v1.xml#L27) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; verified 1.49 surface-relative warp request [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/pointer-warp/pointer-warp-v1.xml#L27) |
| `primary-selection-v1` | Exchange the selection used for middle-click paste | landed; advertised, 1 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/primary-selection/primary-selection-unstable-v1.xml) ; schema lifecycle unstable ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L818) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/selection/primary_selection/mod.rs#L134) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/525) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/docs/CLIPBOARD.md#L38) |
| `relative-pointer-v1` | Deliver motion independent of the visible pointer position | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/relative-pointer/relative-pointer-unstable-v1.xml) ; schema lifecycle unstable ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/relative_pointer.rs#L180) |
| `security-context-v1` | Associate sandbox identity with clients accepted on a listener | landed; advertised, 1; hidden from sandboxed clients | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/security-context/security-context-v1.xml) ; schema lifecycle staging ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L832) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/security_context/mod.rs#L79) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1226) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L478) |
| `single-pixel-buffer-v1` | Create a solid-color single-pixel buffer | unknown; advertised, 1 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/single-pixel-buffer/single-pixel-buffer-v1.xml) ; schema lifecycle staging ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L800) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/single_pixel_buffer/mod.rs#L74) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `tearing-control-v1` | Express an asynchronous presentation preference | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/tearing-control/tearing-control-v1.xml) ; schema lifecycle staging ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `text-input-v1` | Exchange text focus and composition through the earlier text-input protocol | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/text-input/text-input-unstable-v1.xml#L27) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `text-input-v3` | Exchange text focus, surrounding text and composition state | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/text-input/text-input-unstable-v3.xml) ; schema lifecycle unstable ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/text_input/mod.rs#L100) |
| `virtual-keyboard-v1` | Inject keyboard input through a virtual keyboard | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/protocol/virtual-keyboard-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; RemoteDesktop portal input uses EIS or legacy Notify methods as a separate contract [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/remote_desktop.rs#L1) ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/virtual_keyboard/mod.rs#L91) |
| `xwayland-keyboard-grab-v1` | Let an authorized Xwayland client request a keyboard grab | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/xwayland-keyboard-grab/xwayland-keyboard-grab-unstable-v1.xml#L67) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/xwayland_keyboard_grab.rs#L1) ; Smithay xwayland feature-gated; not selected by compositor package manifest [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/Cargo.toml#L84) |
| `xx-session-management-v1` | Carry an experimental session-restoration contract used by Mutter | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/unstable/session-management/xx-session-management-v1.xml#L54) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; exact pinned schema-target name [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/BUILD.gn#L236) |

## Lineage

| Protocol | Date | Documentary finding | Source |
|---|---|---|---|
| `alpha-modifier-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `alpha-modifier-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `alpha-modifier-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `color-management-v1` | 2026-04-11 | Recorded with a deferred disposition [docs] | S-385433e1522c |
| `color-management-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `color-management-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `color-management-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `color-management-v1` | 2026-10-06 | Named `wp_color_manager_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `color-management-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `color-representation-v1` | none | No dated study entry. | none |
| `commit-timing-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `commit-timing-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `content-type-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `content-type-v1` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `content-type-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `content-type-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `content-type-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `cursor-shape-v1` | 2026-04-07 | A dated XML-reference inventory lists this contract among current or earlier files. [docs] | S-b2415d10a041 |
| `cursor-shape-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `cursor-shape-v1` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `cursor-shape-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `cursor-shape-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `cursor-shape-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `cursor-shape-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `cursor-shape-v1` | 2026-10-06 | Named `wp_cursor_shape_manager_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `cursor-shape-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `domicile-anchor-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `drm-lease-v1` | 2026-04-11 | Recorded with a deferred disposition [docs] | S-385433e1522c |
| `drm-lease-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `drm-lease-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `drm-lease-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `drm-lease-v1` | 2026-10-06 | Named `wp_drm_lease_device_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `drm-lease-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `fifo-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `fifo-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `fractional-scale-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `fractional-scale-v1` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `fractional-scale-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `fractional-scale-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `fractional-scale-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `fractional-scale-v1` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `fractional-scale-v1` | 2026-10-06 | Named `wp_fractional_scale_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `fractional-scale-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `fullscreen-shell-v1` | none | No dated study entry. | none |
| `idle-inhibit-v1` | 2026-04-04 | A dated protocol-client inventory lists this wire contract. [docs] | S-88f5036dac2f |
| `idle-inhibit-v1` | 2026-04-07 | A dated XML-reference inventory lists this contract among current or earlier files. [docs] | S-b2415d10a041 |
| `idle-inhibit-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `idle-inhibit-v1` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `idle-inhibit-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `idle-inhibit-v1` | 2026-07-16 | A preserved lock dossier lists this protocol alongside idle and locking contracts. [docs] | S-3e03b7086707 |
| `idle-inhibit-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `idle-inhibit-v1` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `idle-inhibit-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `idle-inhibit-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `input-method-v2` | 2026-04-11 | Recorded with a deferred disposition [docs] | S-385433e1522c |
| `input-method-v2` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `input-method-v2` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `input-method-v2` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `input-method-v2` | 2026-10-06 | Family and generation named separately in a native client study; this entry asserts no complete wire identifier. [docs] | S-e9dc5003d109 |
| `input-method-v2` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `input-timestamps-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `keyboard-shortcuts-inhibit-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `keyboard-shortcuts-inhibit-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `keyboard-shortcuts-inhibit-v1` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `keyboard-shortcuts-inhibit-v1` | 2026-10-06 | Named `zwp_keyboard_shortcuts_inhibit_manager_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `keyboard-shortcuts-inhibit-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `linux-drm-syncobj-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `linux-drm-syncobj-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `linux-drm-syncobj-v1` | 2026-10-06 | Named `wp_linux_drm_syncobj_manager_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `linux-drm-syncobj-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `linux-explicit-synchronization-v1` | none | No dated study entry. | none |
| `pointer-constraints-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `pointer-constraints-v1` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `pointer-constraints-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `pointer-constraints-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `pointer-constraints-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `pointer-constraints-v1` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `pointer-constraints-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `pointer-constraints-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `pointer-gestures-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `pointer-gestures-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `pointer-gestures-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `pointer-gestures-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `pointer-gestures-v1` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `pointer-gestures-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `pointer-gestures-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `pointer-warp-v1` | 2026-04-11 | Recorded with a deferred disposition [docs] | S-385433e1522c |
| `pointer-warp-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `primary-selection-v1` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `primary-selection-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `primary-selection-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `primary-selection-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `relative-pointer-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `relative-pointer-v1` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `relative-pointer-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `relative-pointer-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `relative-pointer-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `relative-pointer-v1` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `relative-pointer-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `relative-pointer-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `security-context-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `security-context-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `security-context-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `security-context-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `security-context-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `security-context-v1` | 2026-10-06 | Named `security_context_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `security-context-v1` | 2026-10-22 | Family-level alias in the source; protocol version not established by this note. [docs] | S-fd840029644f |
| `security-context-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `single-pixel-buffer-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `single-pixel-buffer-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `single-pixel-buffer-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `single-pixel-buffer-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `tearing-control-v1` | 2026-04-11 | Recorded with a deferred disposition [docs] | S-385433e1522c |
| `tearing-control-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `tearing-control-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `tearing-control-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `text-input-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `text-input-v3` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `text-input-v3` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `text-input-v3` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `text-input-v3` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `text-input-v3` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `text-input-v3` | 2026-10-06 | Named `zwp_text_input_manager_v3` in a native client study. [docs] | S-e9dc5003d109 |
| `text-input-v3` | 2026-10-23 | A dated native-toolkit comparison explicitly names this wire contract. [docs] | S-44be82b2b78e |
| `text-input-v3` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `virtual-keyboard-v1` | 2026-04-11 | Recorded with a deferred disposition [docs] | S-385433e1522c |
| `virtual-keyboard-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `virtual-keyboard-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `virtual-keyboard-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `virtual-keyboard-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `virtual-keyboard-v1` | 2026-10-22 | A dated logical-seat source study lists this wire contract. [docs] | S-fd840029644f |
| `virtual-keyboard-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `xwayland-keyboard-grab-v1` | none | No dated study entry. | none |
| `xx-session-management-v1` | none | No dated study entry. | none |

## Sources

- [Complete coverage map](../coverage-map.md).
- S-016ff15271ae · 2026-11-09 · Protocol implementation and registration evidence.
- S-0aa2cc30bb07 · 2026-11-09 · Protocol implementation and registration evidence.
- S-0b6c31dfe6bc · 2026-11-09 · Protocol implementation and registration evidence.
- S-0d6c45f5a62b · 2026-11-09 · Protocol implementation and registration evidence.
- S-1389e89c68c6 · 2026-11-09 · pointer-warp 1.49 XML contract.
- S-18a2678e3352 · 2026-11-09 · color-management 1.49 XML contract.
- S-1c210691c0b8 · 2026-11-09 · Protocol implementation and registration evidence.
- S-2b83da449b94 · 2026-11-09 · Protocol implementation and registration evidence.
- S-2ea0540e0f1d · 2026-04-12 · Protocol implementation and registration evidence.
- S-32784ffd9a8d · 2026-11-09 · Idle activity, blanking and inhibitor contract.
- S-33085e8f5a27 · 2026-11-09 · Smithay input-method manager at the dependency pin.
- S-385433e1522c · 2026-04-11 · Historical protocol audit.
- S-3acbfb4907e7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-3c995b7a34f5 · 2026-11-09 · Protocol implementation and registration evidence.
- S-3d63fd27af2d · 2026-11-09 · Clipboard storage, selection and browser boundary contract.
- S-3e03b7086707 · 2026-07-16 · Protocol implementation and registration evidence.
- S-3e37be57c91a · 2026-11-09 · Protocol implementation and registration evidence.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-4378aebc583f · 2026-11-06 · Projected protocol members and schema coordinates.
- S-43af2b3af183 · 2026-11-09 · Protocol implementation and registration evidence.
- S-44598db24690 · 2026-11-09 · Protocol implementation and registration evidence.
- S-44be82b2b78e · 2026-10-23 · Protocol implementation and registration evidence.
- S-46587779628d · 2026-11-09 · Protocol implementation and registration evidence.
- S-46f8f6ded930 · 2026-11-09 · Protocol implementation and registration evidence.
- S-4aa1e405fd79 · 2026-11-09 · Protocol implementation and registration evidence.
- S-4cd685c56e61 · 2026-11-09 · Protocol implementation and registration evidence.
- S-4d83eed81e29 · 2026-11-09 · Protocol implementation and registration evidence.
- S-534dcadd1369 · 2026-11-09 · Smithay virtual-keyboard handler at the dependency pin.
- S-548e570973fa · 2026-11-09 · Protocol implementation and registration evidence.
- S-5e4ce5d63275 · 2026-10-22 · Summary of a community protocol-page survey.
- S-5f315b39ac7c · 2026-11-09 · Protocol implementation and registration evidence.
- S-66b2ff508daa · 2026-11-09 · Protocol implementation and registration evidence.
- S-67ccf82fd285 · 2026-07-16 · Historical protocol appendix.
- S-703c0d408b29 · 2026-11-09 · Protocol implementation and registration evidence.
- S-70da4c3a822b · 2026-11-08 · Protocol implementation and registration evidence.
- S-7170fa893ec4 · 2026-11-09 · Protocol implementation and registration evidence.
- S-728ccd41be9e · 2026-11-09 · Protocol implementation and registration evidence.
- S-7b31c5bb3be0 · 2026-04-11 · Historical protocol audit.
- S-7f8302736ae8 · 2026-11-09 · Smithay DRM lease handler at the dependency pin.
- S-7fff5f5a9d7c · 2026-11-09 · Protocol implementation and registration evidence.
- S-82b445bd2404 · 2026-11-09 · Protocol implementation and registration evidence.
- S-866b9f3f829d · 2026-11-08 · Public input and desktop compatibility history.
- S-88f5036dac2f · undated · Protocol implementation and registration evidence.
- S-9081893da4f7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-92d9911f0704 · 2026-11-09 · Protocol implementation and registration evidence.
- S-9a9f91803d7e · 2026-11-09 · Smithay text-input manager at the dependency pin.
- S-a04ed2a57dc2 · 2026-11-03 · Source study of an earlier Domicile release.
- S-a19d3732cf8f · 2026-06-11 · Historical protocol audit.
- S-a3671b8e50eb · 2026-11-09 · Protocol implementation and registration evidence.
- S-a542cf9c1edb · 2026-07-16 · Protocol implementation and registration evidence.
- S-a90d2490bf23 · 2026-11-09 · Native gestures, page defaults and shell binding contract.
- S-b2415d10a041 · 2026-04-07 · XML-reference inventory.
- S-b4058c3be3bd · 2026-11-09 · Protocol implementation and registration evidence.
- S-b61833f7b205 · 2026-11-09 · Protocol implementation and registration evidence.
- S-b824688e0e5c · 2026-11-08 · color-representation 1.49 XML contract.
- S-bad77f83f7f6 · 2026-11-09 · Protocol implementation and registration evidence.
- S-c2596d00b100 · 2026-11-09 · Portal interfaces, direct capture and sandbox boundary.
- S-ca6aadf58810 · 2026-11-09 · Protocol implementation and registration evidence.
- S-ccf976691aa5 · 2026-11-09 · Protocol implementation and registration evidence.
- S-ce8dab4e8148 · 2026-11-09 · Protocol implementation and registration evidence.
- S-d2752f2d6c22 · 2026-11-09 · Protocol implementation and registration evidence.
- S-d8f8e61b831b · 2026-11-09 · Protocol implementation and registration evidence.
- S-de5e3879f7ce · 2026-07-16 · Protocol implementation and registration evidence.
- S-e2a9bd2f0d14 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e4ff18df1f16 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e7171507c7aa · 2026-11-09 · Protocol implementation and registration evidence.
- S-e7392f226f69 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e8cd22f8b451 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e9dc5003d109 · 2026-10-06 · Protocol implementation and registration evidence.
- S-ef628d581794 · 2026-11-09 · Protocol implementation and registration evidence.
- S-f41f214e88e8 · 2026-11-09 · Protocol implementation and registration evidence.
- S-f5d75cc3208b · 2026-11-09 · Xdg fullscreen output and activation request routing.
- S-f79b692d55fc · 2026-11-09 · Protocol implementation and registration evidence.
- S-fcc1e24ec465 · 2026-11-09 · Protocol implementation and registration evidence.
- S-fcc50bf0b486 · 2026-11-09 · Protocol implementation and registration evidence.
- S-fd840029644f · 2026-10-22 · Protocol implementation and registration evidence.
- S-ff3fcf31d13d · 2026-11-09 · Protocol implementation and registration evidence.
- S-ff8255d1567e · 2026-11-09 · Protocol implementation and registration evidence.
