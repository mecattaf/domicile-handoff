---
id: protocols/collections/stable
title: stable protocol collection
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
sources: [S-083adea7e7ad, S-2ea0540e0f1d, S-332161a6dfa1, S-385433e1522c, S-3acbfb4907e7, S-3f524db52413, S-4378aebc583f, S-43af2b3af183, S-5bb7550e6b56, S-67ccf82fd285, S-703c0d408b29, S-7b31c5bb3be0, S-98268a478f8c, S-a04ed2a57dc2, S-a19d3732cf8f, S-a542cf9c1edb, S-b32e8bae0905, S-b57d7ad8f1ae, S-b9df7238ba4e, S-bad77f83f7f6, S-bc6dca48e478, S-de5e3879f7ce, S-e7392f226f69, S-e9dc5003d109, S-f37920514535]
---

# stable protocol collection

Stable protocol specifications outside the core wire schema. Collection names group this map; a collection label does not change the specification’s lifecycle or maturity. A dated family or module alias does not establish an exact wire version.

## At the pin

| Protocol | Purpose | Domicile | Evidence and limit |
|---|---|---|---|
| `linux-dmabuf-v1` | Import GPU buffers and describe format/modifier feedback | unknown; advertised, 5 or 3; 5 with default feedback; 3 formats-only; absent without renderer | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/stable/linux-dmabuf/linux-dmabuf-v1.xml) ; schema lifecycle stable ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L713) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/dmabuf/mod.rs#L720) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `presentation-time` | Report when submitted content reaches presentation | unknown; advertised, 2; engine presentation timestamps | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/stable/presentation-time/presentation-time.xml) ; schema lifecycle stable ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L796) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/presentation/mod.rs#L105) ; feedback carries engine time, sequence zero and fixed or unknown refresh [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/frame_pacing.rs#L152) ; an armed deadline answers unreported frame callbacks and discards their presentation feedback after 250 ms [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/frame_pacing.rs#L85); deadline [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/frame_callbacks.rs#L25) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |
| `tablet-v2` | Describe tablet tools, pads, proximity, buttons and axes | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/stable/tablet/tablet-v2.xml) ; schema lifecycle stable ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/tablet_manager/mod.rs#L150) |
| `viewporter` | Crop and scale a surface independently of its buffer size | landed; advertised, 1 | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/stable/viewporter/viewporter.xml) ; schema lifecycle stable ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/viewporter/mod.rs#L89) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/171) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L179) |

## Lineage

| Protocol | Date | Documentary finding | Source |
|---|---|---|---|
| `linux-dmabuf-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `linux-dmabuf-v1` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `linux-dmabuf-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `linux-dmabuf-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `linux-dmabuf-v1` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `linux-dmabuf-v1` | 2026-10-06 | Named `zwp_linux_dmabuf_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `linux-dmabuf-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `linux-dmabuf-v1` | 2026-11-04 | A dated platform trace explicitly names this contract. [docs] | S-b32e8bae0905 |
| `presentation-time` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `presentation-time` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `presentation-time` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `presentation-time` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `presentation-time` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `presentation-time` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `presentation-time` | 2026-10-06 | Named `wp_presentation` in a native client study. [docs] | S-e9dc5003d109 |
| `presentation-time` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `tablet-v2` | 2026-04-11 | Recorded with a deferred disposition [docs] | S-385433e1522c |
| `tablet-v2` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `tablet-v2` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `tablet-v2` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `tablet-v2` | 2026-10-06 | Family and generation named separately in a native client study; this entry asserts no complete wire identifier. [docs] | S-e9dc5003d109 |
| `tablet-v2` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `viewporter` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `viewporter` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `viewporter` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `viewporter` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `viewporter` | 2026-07-16 | A dated server study examined surface and input protocol handling. [docs] | S-de5e3879f7ce |
| `viewporter` | 2026-10-06 | Named `wp_viewporter` in a native client study. [docs] | S-e9dc5003d109 |
| `viewporter` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |

## Sources

- [Complete coverage map](../coverage-map.md).
- S-083adea7e7ad · 2026-11-09 · Protocol implementation and registration evidence.
- S-2ea0540e0f1d · 2026-04-12 · Protocol implementation and registration evidence.
- S-332161a6dfa1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-385433e1522c · 2026-04-11 · Historical protocol audit.
- S-3acbfb4907e7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-4378aebc583f · 2026-11-06 · Projected protocol members and schema coordinates.
- S-43af2b3af183 · 2026-11-09 · Protocol implementation and registration evidence.
- S-5bb7550e6b56 · 2026-11-09 · Protocol implementation and registration evidence.
- S-67ccf82fd285 · 2026-07-16 · Historical protocol appendix.
- S-703c0d408b29 · 2026-11-09 · Protocol implementation and registration evidence.
- S-7b31c5bb3be0 · 2026-04-11 · Historical protocol audit.
- S-98268a478f8c · 2026-11-09 · Protocol implementation and registration evidence.
- S-a04ed2a57dc2 · 2026-11-03 · Source study of an earlier Domicile release.
- S-a19d3732cf8f · 2026-06-11 · Historical protocol audit.
- S-a542cf9c1edb · 2026-07-16 · Protocol implementation and registration evidence.
- S-b32e8bae0905 · 2026-11-04 · Protocol implementation and registration evidence.
- S-b57d7ad8f1ae · 2026-11-09 · Protocol implementation and registration evidence.
- S-b9df7238ba4e · 2026-11-09 · Protocol implementation and registration evidence.
- S-bad77f83f7f6 · 2026-11-09 · Protocol implementation and registration evidence.
- S-bc6dca48e478 · 2026-11-09 · Protocol implementation and registration evidence.
- S-de5e3879f7ce · 2026-07-16 · Protocol implementation and registration evidence.
- S-e7392f226f69 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e9dc5003d109 · 2026-10-06 · Protocol implementation and registration evidence.
- S-f37920514535 · 2026-11-09 · Protocol implementation and registration evidence.
