---
id: protocols/collections/wlr
title: wlr protocol collection
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
sources: [S-1346c57ad82c, S-2ea0540e0f1d, S-385433e1522c, S-3acbfb4907e7, S-3c995b7a34f5, S-3d63fd27af2d, S-3f524db52413, S-4378aebc583f, S-43af2b3af183, S-4aa1e405fd79, S-4fee70ea6452, S-5e4ce5d63275, S-5f315b39ac7c, S-5fb9706b471e, S-67ccf82fd285, S-74f557738f56, S-7cd85cb7ef61, S-82b445bd2404, S-88f5036dac2f, S-8a46216d2a3e, S-914380369a95, S-9586d9691bef, S-9e7d963ab743, S-a04ed2a57dc2, S-a19d3732cf8f, S-a542cf9c1edb, S-b2415d10a041, S-b4058c3be3bd, S-bad77f83f7f6, S-c2596d00b100, S-c34c6ce697a3, S-de5e3879f7ce, S-e0a6e2a8d6f1, S-e7392f226f69, S-e9dc5003d109, S-ef6c7832a511, S-f9c430a954b3, S-fd840029644f]
---

# wlr protocol collection

Protocols originating in the wlroots ecosystem. Collection names group this map; a collection label does not change the specification’s lifecycle or maturity. A dated family or module alias does not establish an exact wire version.

## At the pin

| Protocol | Purpose | Domicile | Evidence and limit |
|---|---|---|---|
| `wlr-data-control-v1` | Allow an authorized clipboard client to read and set selections | landed; advertised, 2; hidden from sandboxed clients | schema [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/protocol/wlr-data-control-unstable-v1.xml) ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L826) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/selection/wlr_data_control/mod.rs#L96) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/950) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/docs/CLIPBOARD.md#L42) |
| `wlr-export-dmabuf-v1` | Export output capture using DMA-BUF descriptors | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/protocol/wlr-export-dmabuf-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; other advertised capture contracts use image-copy and screencopy requests [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/mod.rs#L59) |
| `wlr-foreign-toplevel-management-v1` | List native toplevels and request changes to their state | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/protocol/wlr-foreign-toplevel-management-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; the separate ext list contract advertises read-only toplevel information [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/foreign_toplevels.rs#L28) |
| `wlr-gamma-control-v1` | Set an output gamma lookup table | landed; advertised, 1; hidden from sandboxed clients; gamma-capable engine display required; one holder per display | schema [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/protocol/wlr-gamma-control-unstable-v1.xml) ; a supplied client ramp replaces the night light; release restores it [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/gamma.rs#L7) ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/gamma_control.rs#L25) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1921) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L266) |
| `wlr-layer-shell-v1` | Place native panels and overlays with anchors and exclusive zones | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/protocol/wlr-layer-shell-unstable-v1.xml) ; gtk4-layer-shell 1.3.0 client library requires compositor support for this contract [docs] [source](https://github.com/wmww/gtk4-layer-shell/blob/v1.3.0/README.md#L19) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; the separate custom anchor manager places a native window at a named page anchor [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/anchors.rs#L1) ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/shell/wlr_layer/mod.rs#L1) |
| `wlr-output-management-v1` | Stage, test and apply an output configuration | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/protocol/wlr-output-management-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `wlr-output-power-management-v1` | Request power state changes for an output | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/protocol/wlr-output-power-management-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `wlr-screencopy-v1` | Copy output or region contents into client buffers | landed; advertised, 3; hidden from sandboxed clients | schema [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/protocol/wlr-screencopy-unstable-v1.xml) ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/wlr.rs#L35) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1107) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L501) |
| `wlr-virtual-pointer-v1` | Inject pointer input through a virtual pointer | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/protocol/wlr-virtual-pointer-unstable-v1.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; RemoteDesktop portal input uses EIS or legacy Notify methods as a separate contract [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/remote_desktop.rs#L1) |

## Lineage

| Protocol | Date | Documentary finding | Source |
|---|---|---|---|
| `wlr-data-control-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wlr-data-control-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `wlr-data-control-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `wlr-export-dmabuf-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wlr-export-dmabuf-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `wlr-export-dmabuf-v1` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `wlr-foreign-toplevel-management-v1` | 2026-04-04 | A dated protocol-client inventory lists this wire contract. [docs] | S-88f5036dac2f |
| `wlr-foreign-toplevel-management-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `wlr-foreign-toplevel-management-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wlr-foreign-toplevel-management-v1` | 2026-10-06 | Named `zwlr_foreign_toplevel_management_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `wlr-foreign-toplevel-management-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `wlr-gamma-control-v1` | 2026-04-04 | A dated protocol-client inventory lists this wire contract. [docs] | S-88f5036dac2f |
| `wlr-gamma-control-v1` | 2026-04-07 | A dated XML-reference inventory lists this contract among current or earlier files. [docs] | S-b2415d10a041 |
| `wlr-gamma-control-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `wlr-gamma-control-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `wlr-gamma-control-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wlr-gamma-control-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `wlr-gamma-control-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `wlr-gamma-control-v1` | 2026-10-06 | Named `zwlr_gamma_control_manager_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `wlr-gamma-control-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `wlr-layer-shell-v1` | 2026-04-04 | A dated protocol-client inventory lists this wire contract. [docs] | S-88f5036dac2f |
| `wlr-layer-shell-v1` | 2026-04-07 | A dated XML-reference inventory lists this contract among current or earlier files. [docs] | S-b2415d10a041 |
| `wlr-layer-shell-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `wlr-layer-shell-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `wlr-layer-shell-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wlr-layer-shell-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `wlr-layer-shell-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `wlr-layer-shell-v1` | 2026-10-06 | Named `zwlr_layer_shell_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `wlr-layer-shell-v1` | 2026-10-29 | A dated shell-surface study identifies this contract through an explicit specification link. [docs] | S-4fee70ea6452 |
| `wlr-layer-shell-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `wlr-output-management-v1` | 2026-04-04 | A dated protocol-client inventory lists this wire contract. [docs] | S-88f5036dac2f |
| `wlr-output-management-v1` | 2026-04-07 | A dated XML-reference inventory lists this contract among current or earlier files. [docs] | S-b2415d10a041 |
| `wlr-output-management-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `wlr-output-management-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `wlr-output-management-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wlr-output-management-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `wlr-output-management-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `wlr-output-power-management-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wlr-screencopy-v1` | 2026-04-04 | A dated protocol-client inventory lists this wire contract. [docs] | S-88f5036dac2f |
| `wlr-screencopy-v1` | 2026-04-07 | A dated XML-reference inventory lists this contract among current or earlier files. [docs] | S-b2415d10a041 |
| `wlr-screencopy-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wlr-screencopy-v1` | 2026-10-06 | Named `zwlr_screencopy_manager_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `wlr-screencopy-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `wlr-virtual-pointer-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wlr-virtual-pointer-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `wlr-virtual-pointer-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `wlr-virtual-pointer-v1` | 2026-10-06 | Named `zwlr_virtual_pointer_manager_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `wlr-virtual-pointer-v1` | 2026-10-22 | A dated logical-seat source study lists this wire contract. [docs] | S-fd840029644f |
| `wlr-virtual-pointer-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |

## Sources

- [Complete coverage map](../coverage-map.md).
- S-1346c57ad82c · 2026-11-09 · gtk4-layer-shell 1.3.0 client-library documentation.
- S-2ea0540e0f1d · 2026-04-12 · Protocol implementation and registration evidence.
- S-385433e1522c · 2026-04-11 · Historical protocol audit.
- S-3acbfb4907e7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-3c995b7a34f5 · 2026-11-09 · Protocol implementation and registration evidence.
- S-3d63fd27af2d · 2026-11-09 · Clipboard storage, selection and browser boundary contract.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-4378aebc583f · 2026-11-06 · Projected protocol members and schema coordinates.
- S-43af2b3af183 · 2026-11-09 · Protocol implementation and registration evidence.
- S-4aa1e405fd79 · 2026-11-09 · Protocol implementation and registration evidence.
- S-4fee70ea6452 · 2026-10-29 · Protocol implementation and registration evidence.
- S-5e4ce5d63275 · 2026-10-22 · Summary of a community protocol-page survey.
- S-5f315b39ac7c · 2026-11-09 · Protocol implementation and registration evidence.
- S-5fb9706b471e · 2026-11-09 · Wlr screencopy version registration.
- S-67ccf82fd285 · 2026-07-16 · Historical protocol appendix.
- S-74f557738f56 · 2026-11-09 · Output profiles, pointer transforms and gamma contract.
- S-7cd85cb7ef61 · 2026-11-09 · Protocol implementation and registration evidence.
- S-82b445bd2404 · 2026-11-09 · Protocol implementation and registration evidence.
- S-88f5036dac2f · undated · Protocol implementation and registration evidence.
- S-8a46216d2a3e · 2026-11-09 · Protocol implementation and registration evidence.
- S-914380369a95 · 2026-11-09 · Protocol implementation and registration evidence.
- S-9586d9691bef · 2026-11-09 · Protocol implementation and registration evidence.
- S-9e7d963ab743 · 2026-11-09 · Protocol implementation and registration evidence.
- S-a04ed2a57dc2 · 2026-11-03 · Source study of an earlier Domicile release.
- S-a19d3732cf8f · 2026-06-11 · Historical protocol audit.
- S-a542cf9c1edb · 2026-07-16 · Protocol implementation and registration evidence.
- S-b2415d10a041 · 2026-04-07 · XML-reference inventory.
- S-b4058c3be3bd · 2026-11-09 · Protocol implementation and registration evidence.
- S-bad77f83f7f6 · 2026-11-09 · Protocol implementation and registration evidence.
- S-c2596d00b100 · 2026-11-09 · Portal interfaces, direct capture and sandbox boundary.
- S-c34c6ce697a3 · 2026-11-09 · Protocol implementation and registration evidence.
- S-de5e3879f7ce · 2026-07-16 · Protocol implementation and registration evidence.
- S-e0a6e2a8d6f1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e7392f226f69 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e9dc5003d109 · 2026-10-06 · Protocol implementation and registration evidence.
- S-ef6c7832a511 · 2026-11-09 · Image-copy and capture-source manager registration.
- S-f9c430a954b3 · 2026-11-09 · Per-output gamma-control manager registration.
- S-fd840029644f · 2026-10-22 · Protocol implementation and registration evidence.
