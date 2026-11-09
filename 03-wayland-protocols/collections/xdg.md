---
id: protocols/collections/xdg
title: xdg protocol collection
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
sources: [S-014c456bb822, S-1497ad1e40a4, S-14b8d68070ec, S-1f5025ab76ad, S-229f7c4d9b14, S-2822956bdf13, S-2ea0540e0f1d, S-3674591ca8e6, S-385433e1522c, S-3acbfb4907e7, S-3f524db52413, S-3f5dbd0c9580, S-4378aebc583f, S-43af2b3af183, S-499a1aa22127, S-51497eee422b, S-5532dadfd152, S-5e57a72fa9cc, S-654a5c549cb3, S-67ccf82fd285, S-687d7a1db0ce, S-7a89a244a0f8, S-7b295c5a7393, S-7b31c5bb3be0, S-863c5efcb1a1, S-a04ed2a57dc2, S-a19d3732cf8f, S-a542cf9c1edb, S-a6589c3205d1, S-bad77f83f7f6, S-bd39d7cfd9cb, S-c2596d00b100, S-ca1441e8ae06, S-cecb234a3206, S-d2752f2d6c22, S-d6227f9723b1, S-de5e3879f7ce, S-e7392f226f69, S-e77ffff40807, S-e8cd22f8b451, S-e9dc5003d109, S-f66da6ee5bfa]
---

# xdg protocol collection

Desktop-shell, activation and metadata protocols. Collection names group this map; a collection label does not change the specification’s lifecycle or maturity. A dated family or module alias does not establish an exact wire version.

## At the pin

| Protocol | Purpose | Domicile | Evidence and limit |
|---|---|---|---|
| `xdg-activation-v1` | Transfer an activation request using a token | landed; advertised, 1 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/xdg-activation/xdg-activation-v1.xml) ; schema lifecycle staging ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1274) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/xdg_activation/mod.rs#L187) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1040) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1778) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L616) |
| `xdg-decoration-v1` | Negotiate client or server window decorations | unknown; advertised, 1 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/xdg-decoration/xdg-decoration-unstable-v1.xml) ; schema lifecycle unstable ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1275) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/shell/xdg/decoration.rs#L146) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `xdg-dialog-v1` | Describe dialog modality relative to a toplevel | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/xdg-dialog/xdg-dialog-v1.xml#L26) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/shell/xdg/dialog.rs#L106) |
| `xdg-foreign-v1` | Export and import a cross-client parent relationship | landed; advertised, 1; exporter only; importer global omitted at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/xdg-foreign/xdg-foreign-unstable-v1.xml) ; schema lifecycle unstable ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/xdg_foreign.rs#L22) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/952) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L87) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L143) |
| `xdg-foreign-v2` | Export and import a cross-client parent relationship with revised lifetime rules | landed; advertised, 1; exporter only; importer global omitted at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/xdg-foreign/xdg-foreign-unstable-v2.xml) ; schema lifecycle unstable ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/xdg_foreign.rs#L23) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/952) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L87) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L143) |
| `xdg-output-v1` | Describe logical output geometry and identity | unknown; advertised, 3 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/unstable/xdg-output/xdg-output-unstable-v1.xml) ; schema lifecycle unstable ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L874) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/output/mod.rs#L132) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `xdg-session-management-v1` | Associate native toplevels with sessions and request state restoration | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/xdg-session-management/xdg-session-management-v1.xml#L60) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; verified 1.49 schema and restoration scope [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/xdg-session-management/xdg-session-management-v1.xml#L60) ; remembered state and eviction policy are compositor choices in this contract |
| `xdg-shell` | Manage native toplevel and popup roles and configure cycles | unknown; advertised, 6 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/stable/xdg-shell/xdg-shell.xml) ; schema lifecycle stable ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1273) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/shell/xdg/mod.rs#L1252) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `xdg-system-bell-v1` | Request a system bell associated with a surface | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/xdg-system-bell/xdg-system-bell-v1.xml#L26) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/xdg_system_bell.rs#L53) |
| `xdg-toplevel-drag-v1` | Associate a native toplevel move with a drag-and-drop session | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/xdg-toplevel-drag/xdg-toplevel-drag-v1.xml#L27) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; exact pinned schema-target name [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/BUILD.gn#L228) |
| `xdg-toplevel-icon-v1` | Supply an icon for an individual native toplevel | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/xdg-toplevel-icon/xdg-toplevel-icon-v1.xml#L46) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/xdg_toplevel_icon.rs#L223) |
| `xdg-toplevel-tag-v1` | Attach an instance tag and description to a toplevel | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/xdg-toplevel-tag/xdg-toplevel-tag-v1.xml) ; schema lifecycle staging ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/xdg_toplevel_tag.rs#L110) |
| `xwayland-shell-v1` | Associate an Xwayland surface with its X11 window | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/xwayland-shell/xwayland-shell-v1.xml#L59) ; sway private-client global filter [source] [sway](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/server.c#L136) ; scroll private-client global filter [source] [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/server.c#L137) ; Scroll bind rejects other clients [source] [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/xwayland/shell.c#L143) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/xwayland_shell.rs#L1) ; Smithay xwayland feature-gated; not selected by compositor package manifest [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/Cargo.toml#L84) |

## Lineage

| Protocol | Date | Documentary finding | Source |
|---|---|---|---|
| `xdg-activation-v1` | 2026-04-11 | Cataloged in a historical protocol audit. [docs] | S-385433e1522c |
| `xdg-activation-v1` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `xdg-activation-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `xdg-activation-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `xdg-activation-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `xdg-activation-v1` | 2026-10-06 | Named `xdg_activation_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `xdg-activation-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `xdg-decoration-v1` | 2026-04-11 | Cataloged in a historical protocol audit. [docs] | S-385433e1522c |
| `xdg-decoration-v1` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `xdg-decoration-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `xdg-decoration-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `xdg-decoration-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `xdg-decoration-v1` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `xdg-decoration-v1` | 2026-10-06 | Named `zxdg_decoration_manager_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `xdg-decoration-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `xdg-dialog-v1` | 2026-10-06 | Named `xdg_dialog_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `xdg-dialog-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `xdg-foreign-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `xdg-foreign-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `xdg-foreign-v2` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `xdg-foreign-v2` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `xdg-foreign-v2` | 2026-10-06 | Family and generation named separately in a native client study; this entry asserts no complete wire identifier. [docs] | S-e9dc5003d109 |
| `xdg-foreign-v2` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `xdg-output-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `xdg-output-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `xdg-output-v1` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `xdg-output-v1` | 2026-10-06 | Named `zxdg_output_manager_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `xdg-output-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `xdg-session-management-v1` | none | No dated study entry. | none |
| `xdg-shell` | 2026-04-11 | Cataloged in a historical protocol audit. [docs] | S-385433e1522c |
| `xdg-shell` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `xdg-shell` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `xdg-shell` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `xdg-shell` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `xdg-shell` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `xdg-shell` | 2026-10-06 | Named `xdg_wm_base` in a native client study. [docs] | S-e9dc5003d109 |
| `xdg-shell` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `xdg-system-bell-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `xdg-system-bell-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `xdg-toplevel-drag-v1` | 2026-10-06 | An archived native-client comparison names this wire protocol. [docs] | S-e9dc5003d109 |
| `xdg-toplevel-icon-v1` | 2026-10-06 | Named `xdg_toplevel_icon_manager_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `xdg-toplevel-icon-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `xdg-toplevel-tag-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `xwayland-shell-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `xwayland-shell-v1` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `xwayland-shell-v1` | 2026-10-06 | Named `xwayland_shell_v1` in a native client study. [docs] | S-e9dc5003d109 |

## Sources

- [Complete coverage map](../coverage-map.md).
- S-014c456bb822 · 2026-11-09 · Protocol implementation and registration evidence.
- S-1497ad1e40a4 · 2026-11-09 · Protocol implementation and registration evidence.
- S-14b8d68070ec · 2026-11-09 · Protocol implementation and registration evidence.
- S-1f5025ab76ad · 2026-11-09 · Protocol implementation and registration evidence.
- S-229f7c4d9b14 · 2026-11-09 · Protocol implementation and registration evidence.
- S-2822956bdf13 · 2026-11-09 · Protocol implementation and registration evidence.
- S-2ea0540e0f1d · 2026-04-12 · Protocol implementation and registration evidence.
- S-3674591ca8e6 · 2026-11-09 · xdg-session-management 1.49 XML contract.
- S-385433e1522c · 2026-04-11 · Historical protocol audit.
- S-3acbfb4907e7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-3f5dbd0c9580 · 2026-11-09 · Protocol implementation and registration evidence.
- S-4378aebc583f · 2026-11-06 · Projected protocol members and schema coordinates.
- S-43af2b3af183 · 2026-11-09 · Protocol implementation and registration evidence.
- S-499a1aa22127 · 2026-11-09 · Niri 25.08 external X11-to-Wayland bridge documentation.
- S-51497eee422b · 2026-11-09 · Protocol implementation and registration evidence.
- S-5532dadfd152 · 2026-11-09 · Protocol implementation and registration evidence.
- S-5e57a72fa9cc · 2026-11-09 · Protocol implementation and registration evidence.
- S-654a5c549cb3 · 2026-11-09 · Protocol implementation and registration evidence.
- S-67ccf82fd285 · 2026-07-16 · Historical protocol appendix.
- S-687d7a1db0ce · 2026-11-09 · Protocol implementation and registration evidence.
- S-7a89a244a0f8 · 2026-11-09 · Protocol implementation and registration evidence.
- S-7b295c5a7393 · 2026-11-09 · Protocol implementation and registration evidence.
- S-7b31c5bb3be0 · 2026-04-11 · Historical protocol audit.
- S-863c5efcb1a1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-a04ed2a57dc2 · 2026-11-03 · Source study of an earlier Domicile release.
- S-a19d3732cf8f · 2026-06-11 · Historical protocol audit.
- S-a542cf9c1edb · 2026-07-16 · Protocol implementation and registration evidence.
- S-a6589c3205d1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-bad77f83f7f6 · 2026-11-09 · Protocol implementation and registration evidence.
- S-bd39d7cfd9cb · 2026-11-09 · Protocol implementation and registration evidence.
- S-c2596d00b100 · 2026-11-09 · Portal interfaces, direct capture and sandbox boundary.
- S-ca1441e8ae06 · 2026-11-09 · Protocol implementation and registration evidence.
- S-cecb234a3206 · 2026-11-09 · Protocol implementation and registration evidence.
- S-d2752f2d6c22 · 2026-11-09 · Protocol implementation and registration evidence.
- S-d6227f9723b1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-de5e3879f7ce · 2026-07-16 · Protocol implementation and registration evidence.
- S-e7392f226f69 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e77ffff40807 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e8cd22f8b451 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e9dc5003d109 · 2026-10-06 · Protocol implementation and registration evidence.
- S-f66da6ee5bfa · 2026-11-09 · Protocol implementation and registration evidence.
