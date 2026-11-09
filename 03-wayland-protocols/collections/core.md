---
id: protocols/collections/core
title: core protocol collection
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
sources: [S-1570b41820c4, S-291dce183eb2, S-2ea0540e0f1d, S-385433e1522c, S-3acbfb4907e7, S-3f524db52413, S-4378aebc583f, S-43af2b3af183, S-472d3a68890f, S-507aa315e372, S-5532dadfd152, S-594f7c37cd88, S-5eb8003b42fd, S-66930fd17085, S-67ccf82fd285, S-6ba440ed6420, S-77d28cc4fbfe, S-782b0928e349, S-7b31c5bb3be0, S-832c3939e13e, S-9175c1754b40, S-9ecc381476d7, S-a04ed2a57dc2, S-a19d3732cf8f, S-a1fd4d71b416, S-a542cf9c1edb, S-a90d2490bf23, S-ae7fe663e21e, S-af6e4774be6e, S-bad77f83f7f6, S-ca1441e8ae06, S-cc860987e54f, S-de5e3879f7ce, S-e52ad911c3ac, S-e7392f226f69, S-e8cd22f8b451, S-e9dc5003d109, S-fac59fa61d8f, S-fd840029644f]
---

# core protocol collection

Core objects and seat-created input resources. Collection names group this map; a collection label does not change the specification’s lifecycle or maturity. A dated family or module alias does not establish an exact wire version.

## At the pin

| Protocol | Purpose | Domicile | Evidence and limit |
|---|---|---|---|
| `wl_compositor` | Create surfaces and regions; commit buffered surface state | unknown; advertised, 5 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland/-/blob/1.26.0/protocol/wayland.xml) ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1272) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/compositor/mod.rs#L684) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `wl_data_device_manager` | Exchange clipboard selections and drag-and-drop offers | unknown; advertised, 3 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland/-/blob/1.26.0/protocol/wayland.xml) ; native MIME offers are preserved by the Smithay selection path [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/selection/data_device/source.rs#L67) ; the separate engine/history snapshot selects text and image MIME types [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/selections.rs#L65) ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L817) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/selection/data_device/mod.rs#L207) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `wl_fixes` | Coordinate safe registry/global removal | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland/-/blob/1.26.0/protocol/wayland.xml) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `wl_keyboard` | Deliver keyboard maps, focus, keys, modifiers and repeat information | unknown; resource via advertised wl_seat, 9 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland/-/blob/1.26.0/protocol/wayland.xml) ; seat-created resource; inherited version ceiling 9 [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L834) ; capability present [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L844) ; touch always present with pointer fallback [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L864) ; seat resource dispatch [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/seat.rs#L78) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `wl_output` | Describe an output geometry, mode, scale and identity | unknown; advertised, 4; one global per advertised display | schema [schema](https://gitlab.freedesktop.org/wayland/wayland/-/blob/1.26.0/protocol/wayland.xml) ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/outputs.rs#L78) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/output/mod.rs#L208) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `wl_pointer` | Deliver pointer motion, buttons, axis data and cursor requests | unknown; resource via advertised wl_seat, 9 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland/-/blob/1.26.0/protocol/wayland.xml) ; seat-created resource; inherited version ceiling 9 [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L834) ; capability present [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L844) ; touch always present with pointer fallback [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L864) ; seat resource dispatch [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/seat.rs#L78) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `wl_seat` | Advertise a logical input seat and its device capabilities | unknown; advertised, 9; one seat; pointer, keyboard and touch capabilities | schema [schema](https://gitlab.freedesktop.org/wayland/wayland/-/blob/1.26.0/protocol/wayland.xml) ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L834) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/seat/mod.rs#L176) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `wl_shell` | Provide the deprecated core shell surface role | absent; not advertised; client cannot bind a global for this wire contract at the pin | additional schema declarations [source] [schema](https://gitlab.freedesktop.org/wayland/wayland/-/blob/1.26.0/protocol/wayland.xml#L1153) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; xdg-shell remains advertised as the native window role [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1273) |
| `wl_shm` | Supply pixel buffers through shared memory | unknown; advertised, 2 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland/-/blob/1.26.0/protocol/wayland.xml) ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1283) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/shm/mod.rs#L157) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `wl_subcompositor` | Create child surfaces with position and synchronization state | unknown; advertised, 1 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland/-/blob/1.26.0/protocol/wayland.xml) ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1272) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/compositor/mod.rs#L706) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `wl_touch` | Deliver contact sequences, cancellation, frames, shape and orientation | landed; resource via advertised wl_seat, 9 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland/-/blob/1.26.0/protocol/wayland.xml) ; seat-created resource; inherited version ceiling 9 [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L834) ; capability present [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L844) ; touch always present with pointer fallback [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L864) ; seat resource dispatch [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/seat.rs#L78) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1924) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L51) |

## Lineage

| Protocol | Date | Documentary finding | Source |
|---|---|---|---|
| `wl_compositor` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `wl_compositor` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `wl_compositor` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wl_compositor` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `wl_compositor` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `wl_compositor` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `wl_compositor` | 2026-10-06 | Named `wl_compositor` in a native client study. [docs] | S-e9dc5003d109 |
| `wl_compositor` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `wl_data_device_manager` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `wl_data_device_manager` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `wl_data_device_manager` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `wl_data_device_manager` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wl_data_device_manager` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `wl_data_device_manager` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `wl_data_device_manager` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `wl_data_device_manager` | 2026-10-06 | Named `wl_data_device_manager` in a native client study. [docs] | S-e9dc5003d109 |
| `wl_data_device_manager` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `wl_fixes` | 2026-10-06 | Named `wl_fixes` in a native client study. [docs] | S-e9dc5003d109 |
| `wl_keyboard` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `wl_keyboard` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wl_keyboard` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `wl_keyboard` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `wl_output` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `wl_output` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `wl_output` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wl_output` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `wl_output` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `wl_output` | 2026-10-06 | Named `wl_output` in a native client study. [docs] | S-e9dc5003d109 |
| `wl_output` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `wl_pointer` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `wl_pointer` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wl_pointer` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `wl_pointer` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `wl_seat` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `wl_seat` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `wl_seat` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wl_seat` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `wl_seat` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `wl_seat` | 2026-10-06 | Named `wl_seat` in a native client study. [docs] | S-e9dc5003d109 |
| `wl_seat` | 2026-10-22 | An archived transport study names the core seat interface. [docs] | S-fac59fa61d8f |
| `wl_seat` | 2026-10-22 | A dated logical-seat source study lists this wire contract. [docs] | S-fd840029644f |
| `wl_seat` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `wl_shell` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wl_shell` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `wl_shm` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `wl_shm` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `wl_shm` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wl_shm` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `wl_shm` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `wl_shm` | 2026-10-06 | Named `wl_shm` in a native client study. [docs] | S-e9dc5003d109 |
| `wl_shm` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `wl_subcompositor` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `wl_subcompositor` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `wl_subcompositor` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wl_subcompositor` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `wl_subcompositor` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `wl_subcompositor` | 2026-10-06 | Named `wl_subcompositor` in a native client study. [docs] | S-e9dc5003d109 |
| `wl_touch` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `wl_touch` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `wl_touch` | 2026-10-06 | Named `wl_touch` in a native client study. [docs] | S-e9dc5003d109 |
| `wl_touch` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |

## Sources

- [Complete coverage map](../coverage-map.md).
- S-1570b41820c4 · 2026-11-09 · Protocol implementation and registration evidence.
- S-291dce183eb2 · 2026-11-09 · Smithay data-device manager and default drag callbacks.
- S-2ea0540e0f1d · 2026-04-12 · Protocol implementation and registration evidence.
- S-385433e1522c · 2026-04-11 · Historical protocol audit.
- S-3acbfb4907e7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-4378aebc583f · 2026-11-06 · Projected protocol members and schema coordinates.
- S-43af2b3af183 · 2026-11-09 · Protocol implementation and registration evidence.
- S-472d3a68890f · 2026-11-09 · Protocol implementation and registration evidence.
- S-507aa315e372 · 2026-11-09 · Protocol implementation and registration evidence.
- S-5532dadfd152 · 2026-11-09 · Protocol implementation and registration evidence.
- S-594f7c37cd88 · 2026-11-09 · Protocol implementation and registration evidence.
- S-5eb8003b42fd · 2026-11-09 · Protocol implementation and registration evidence.
- S-66930fd17085 · 2026-11-09 · Protocol implementation and registration evidence.
- S-67ccf82fd285 · 2026-07-16 · Historical protocol appendix.
- S-6ba440ed6420 · 2026-11-09 · Protocol implementation and registration evidence.
- S-77d28cc4fbfe · 2026-11-09 · Protocol implementation and registration evidence.
- S-782b0928e349 · 2026-11-09 · Protocol implementation and registration evidence.
- S-7b31c5bb3be0 · 2026-04-11 · Historical protocol audit.
- S-832c3939e13e · 2026-11-09 · Protocol implementation and registration evidence.
- S-9175c1754b40 · 2026-11-09 · Wayland output creation and fractional-scale reporting.
- S-9ecc381476d7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-a04ed2a57dc2 · 2026-11-03 · Source study of an earlier Domicile release.
- S-a19d3732cf8f · 2026-06-11 · Historical protocol audit.
- S-a1fd4d71b416 · 2026-11-09 · Protocol implementation and registration evidence.
- S-a542cf9c1edb · 2026-07-16 · Protocol implementation and registration evidence.
- S-a90d2490bf23 · 2026-11-09 · Native gestures, page defaults and shell binding contract.
- S-ae7fe663e21e · 2026-11-09 · Protocol implementation and registration evidence.
- S-af6e4774be6e · 2026-11-09 · Protocol implementation and registration evidence.
- S-bad77f83f7f6 · 2026-11-09 · Protocol implementation and registration evidence.
- S-ca1441e8ae06 · 2026-11-09 · Protocol implementation and registration evidence.
- S-cc860987e54f · 2026-11-09 · Protocol schema inventory.
- S-de5e3879f7ce · 2026-07-16 · Protocol implementation and registration evidence.
- S-e52ad911c3ac · 2026-11-09 · Domicile drag callbacks and selection serving.
- S-e7392f226f69 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e8cd22f8b451 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e9dc5003d109 · 2026-10-06 · Protocol implementation and registration evidence.
- S-fac59fa61d8f · 2026-10-22 · Protocol implementation and registration evidence.
- S-fd840029644f · 2026-10-22 · Protocol implementation and registration evidence.
