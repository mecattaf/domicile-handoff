---
id: protocols/collections/ext
title: ext protocol collection
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
sources: [S-00ffae850184, S-1346c57ad82c, S-2ea0540e0f1d, S-385433e1522c, S-3acbfb4907e7, S-3d63fd27af2d, S-3e03b7086707, S-3f524db52413, S-4378aebc583f, S-43af2b3af183, S-4aa1e405fd79, S-5e4ce5d63275, S-67ccf82fd285, S-71c6b5b6fa74, S-7b31c5bb3be0, S-82b445bd2404, S-85c7cd8374aa, S-88f5036dac2f, S-8a46216d2a3e, S-8e9e08836f9d, S-a04ed2a57dc2, S-a103e36fe4b1, S-a19d3732cf8f, S-a542cf9c1edb, S-b2415d10a041, S-b32e8bae0905, S-b4058c3be3bd, S-bad77f83f7f6, S-c2596d00b100, S-c34c6ce697a3, S-c4bef886de89, S-c4fb695b7c41, S-c5d3db1884f1, S-d2752f2d6c22, S-e0a6e2a8d6f1, S-e7392f226f69, S-e9dc5003d109, S-ef6c7832a511, S-fd840029644f]
---

# ext protocol collection

Ext-prefixed desktop contracts and historical draft names, with lifecycle recorded per row. Collection names group this map; a collection label does not change the specification’s lifecycle or maturity. A dated family or module alias does not establish an exact wire version.

## At the pin

| Protocol | Purpose | Domicile | Evidence and limit |
|---|---|---|---|
| `ext-background-effect-v1` | Attach background-effect state to a surface | absent; not advertised; client cannot bind a global for this wire contract at the pin | Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `ext-data-control-v1` | Allow an authorized clipboard client to read and set selections | landed; advertised, 1; hidden from sandboxed clients | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/ext-data-control/ext-data-control-v1.xml) ; schema lifecycle staging ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L821) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/selection/ext_data_control/mod.rs#L92) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/950) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/docs/CLIPBOARD.md#L42) |
| `ext-foreign-toplevel-image-capture-source-v1` | Name a listed toplevel as a capture source | landed; advertised, 1; hidden from sandboxed clients | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/ext-image-capture-source/ext-image-capture-source-v1.xml) ; schema lifecycle staging ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/mod.rs#L61) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1510) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L501) |
| `ext-foreign-toplevel-list-v1` | List native toplevel identity, title and application id | landed; advertised, 1; hidden from sandboxed clients | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/ext-foreign-toplevel-list/ext-foreign-toplevel-list-v1.xml) ; schema lifecycle staging ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/foreign_toplevels.rs#L28) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/foreign_toplevel_list/mod.rs#L303) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1510) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L501) |
| `ext-idle-notify-v1` | Notify a client after an inactivity interval | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/ext-idle-notify/ext-idle-notify-v1.xml) ; schema lifecycle staging ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; internal idle timers and inhibitors are a separate available route [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/idle.rs#L1) ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/idle_notify/mod.rs#L119) |
| `ext-image-capture-source-v1` | Provide output and toplevel source objects for image capture | landed; source objects via advertised output/toplevel managers, 1; umbrella schema, no distinct global | umbrella schema source objects and two manager globals [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/mod.rs#L26) ; both source-manager registrations [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/mod.rs#L59) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1107) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1510) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L501) |
| `ext-image-copy-capture-v1` | Copy a capture source into client buffers with cursor and damage information | landed; advertised, 1; hidden from sandboxed clients | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/ext-image-copy-capture/ext-image-copy-capture-v1.xml) ; schema lifecycle staging ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/mod.rs#L65) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1107) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L501) |
| `ext-output-image-capture-source-v1` | Name an output as a capture source | landed; advertised, 1; hidden from sandboxed clients | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/ext-image-capture-source/ext-image-capture-source-v1.xml) ; schema lifecycle staging ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/mod.rs#L60) ; merged PR [source] [source](https://github.com/cprussin/domicile/pull/1107) ; pinned protocol/capability documentation [docs] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L501) |
| `ext-session-lock-v1` | Give a lock client exclusive lock surfaces and input | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/ext-session-lock/ext-session-lock-v1.xml) ; schema lifecycle staging ; gtk4-layer-shell 1.3.0 documents client bindings for this lock contract [docs] [source](https://github.com/wmww/gtk4-layer-shell/blob/v1.3.0/README.md#L5) ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; the compositor has its own enforced lock with a shell-drawn screen [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/lock.rs#L7) ; Smithay handler source available [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/session_lock/mod.rs#L91) |
| `ext-session-management-v1` | Historical draft name for session restoration; exact schema identity unverified | unknown; historical draft name; schema identity unverified | Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition ; historical draft spelling only [docs] S-385433e1522c; distinct experimental xx-session-management target [source] [Exo](https://github.com/chromium/chromium/blob/150.0.7871.124/third_party/wayland-protocols/BUILD.gn#L236) |
| `ext-transient-seat-v1` | Request a temporary logical seat | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/ext-transient-seat/ext-transient-seat-v1.xml) ; schema lifecycle staging ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |
| `ext-workspace-v1` | List workspace groups and workspaces with associated requests | absent; not advertised; client cannot bind a global for this wire contract at the pin | schema [schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/ext-workspace/ext-workspace-v1.xml) ; schema lifecycle staging ; Compositor registration audit 2026-11-09 at 996d8b628439a5d9b12204b49c9d472f249666bc, 151 non-bin Rust modules; client binaries excluded [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793) ; absence is a disposition |

## Lineage

| Protocol | Date | Documentary finding | Source |
|---|---|---|---|
| `ext-background-effect-v1` | 2026-04-04 | A dated protocol-client inventory lists this wire contract. [docs] | S-88f5036dac2f |
| `ext-background-effect-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `ext-background-effect-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `ext-background-effect-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `ext-background-effect-v1` | 2026-10-06 | Named `ext_background_effect_manager_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `ext-data-control-v1` | 2026-04-04 | A dated protocol-client inventory lists this wire contract. [docs] | S-88f5036dac2f |
| `ext-data-control-v1` | 2026-04-07 | A dated XML-reference inventory lists this contract among current or earlier files. [docs] | S-b2415d10a041 |
| `ext-data-control-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `ext-data-control-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `ext-data-control-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `ext-data-control-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `ext-data-control-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `ext-data-control-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `ext-data-control-v1` | 2026-11-04 | A dated platform trace explicitly names this contract. [docs] | S-b32e8bae0905 |
| `ext-foreign-toplevel-image-capture-source-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `ext-foreign-toplevel-image-capture-source-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `ext-foreign-toplevel-image-capture-source-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `ext-foreign-toplevel-list-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `ext-foreign-toplevel-list-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `ext-foreign-toplevel-list-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `ext-foreign-toplevel-list-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `ext-foreign-toplevel-list-v1` | 2026-10-06 | Named `ext_foreign_toplevel_list_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `ext-foreign-toplevel-list-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `ext-idle-notify-v1` | 2026-04-04 | A dated protocol-client inventory lists this wire contract. [docs] | S-88f5036dac2f |
| `ext-idle-notify-v1` | 2026-04-07 | A dated XML-reference inventory lists this contract among current or earlier files. [docs] | S-b2415d10a041 |
| `ext-idle-notify-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `ext-idle-notify-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `ext-idle-notify-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `ext-idle-notify-v1` | 2026-07-16 | A preserved lock dossier lists this protocol alongside idle and locking contracts. [docs] | S-3e03b7086707 |
| `ext-idle-notify-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `ext-idle-notify-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `ext-idle-notify-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `ext-image-capture-source-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `ext-image-capture-source-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `ext-image-capture-source-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `ext-image-capture-source-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `ext-image-copy-capture-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `ext-image-copy-capture-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `ext-image-copy-capture-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `ext-image-copy-capture-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `ext-image-copy-capture-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `ext-image-copy-capture-v1` | 2026-10-06 | Family or module mentioned in a native client study; this entry makes no version-specific claim. [docs] | S-e9dc5003d109 |
| `ext-image-copy-capture-v1` | 2026-10-22 | Family-level alias in the source; protocol version not established by this note. [docs] | S-fd840029644f |
| `ext-image-copy-capture-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `ext-output-image-capture-source-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `ext-output-image-capture-source-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `ext-output-image-capture-source-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `ext-session-lock-v1` | 2026-04-04 | A dated protocol-client inventory lists this wire contract. [docs] | S-88f5036dac2f |
| `ext-session-lock-v1` | 2026-04-07 | A dated XML-reference inventory lists this contract among current or earlier files. [docs] | S-b2415d10a041 |
| `ext-session-lock-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `ext-session-lock-v1` | 2026-04-11 | A dated native-client surface study lists this wire contract. [docs] | S-7b31c5bb3be0 |
| `ext-session-lock-v1` | 2026-04-12 | A dated parity annotation discussed this protocol contract. [docs] | S-2ea0540e0f1d |
| `ext-session-lock-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `ext-session-lock-v1` | 2026-07-16 | A preserved lock dossier lists this protocol alongside idle and locking contracts. [docs] | S-3e03b7086707 |
| `ext-session-lock-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `ext-session-lock-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `ext-session-lock-v1` | 2026-10-06 | Named `ext_session_lock_v1` in a native client study. [docs] | S-e9dc5003d109 |
| `ext-session-lock-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |
| `ext-session-management-v1` | 2026-04-11 | Recorded with a deferred disposition [docs] | S-385433e1522c |
| `ext-transient-seat-v1` | 2026-04-11 | Recorded with a deferred disposition [docs] | S-385433e1522c |
| `ext-transient-seat-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `ext-transient-seat-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `ext-workspace-v1` | 2026-04-04 | A dated protocol-client inventory lists this wire contract. [docs] | S-88f5036dac2f |
| `ext-workspace-v1` | 2026-04-11 | Cataloged in the 2026-04-11 audit [docs] | S-385433e1522c |
| `ext-workspace-v1` | 2026-06-11 | June protocol research examined this contract. [docs] | S-a19d3732cf8f |
| `ext-workspace-v1` | 2026-07-16 | A dated protocol catalog included this contract. [docs] | S-67ccf82fd285 |
| `ext-workspace-v1` | 2026-07-16 | A dated catalog recorded a native-client protocol entry. [docs] | S-a542cf9c1edb |
| `ext-workspace-v1` | 2026-10-06 | Named `ext-workspace-v1` in a native client study. [docs] | S-e9dc5003d109 |
| `ext-workspace-v1` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |

## Sources

- [Complete coverage map](../coverage-map.md).
- S-00ffae850184 · 2026-11-09 · Protocol implementation and registration evidence.
- S-1346c57ad82c · 2026-11-09 · gtk4-layer-shell 1.3.0 client-library documentation.
- S-2ea0540e0f1d · 2026-04-12 · Protocol implementation and registration evidence.
- S-385433e1522c · 2026-04-11 · Historical protocol audit.
- S-3acbfb4907e7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-3d63fd27af2d · 2026-11-09 · Clipboard storage, selection and browser boundary contract.
- S-3e03b7086707 · 2026-07-16 · Protocol implementation and registration evidence.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-4378aebc583f · 2026-11-06 · Projected protocol members and schema coordinates.
- S-43af2b3af183 · 2026-11-09 · Protocol implementation and registration evidence.
- S-4aa1e405fd79 · 2026-11-09 · Protocol implementation and registration evidence.
- S-5e4ce5d63275 · 2026-10-22 · Summary of a community protocol-page survey.
- S-67ccf82fd285 · 2026-07-16 · Historical protocol appendix.
- S-71c6b5b6fa74 · 2026-11-09 · Protocol implementation and registration evidence.
- S-7b31c5bb3be0 · 2026-04-11 · Historical protocol audit.
- S-82b445bd2404 · 2026-11-09 · Protocol implementation and registration evidence.
- S-85c7cd8374aa · 2026-11-09 · Protocol implementation and registration evidence.
- S-88f5036dac2f · undated · Protocol implementation and registration evidence.
- S-8a46216d2a3e · 2026-11-09 · Protocol implementation and registration evidence.
- S-8e9e08836f9d · 2026-11-09 · Protocol implementation and registration evidence.
- S-a04ed2a57dc2 · 2026-11-03 · Source study of an earlier Domicile release.
- S-a103e36fe4b1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-a19d3732cf8f · 2026-06-11 · Historical protocol audit.
- S-a542cf9c1edb · 2026-07-16 · Protocol implementation and registration evidence.
- S-b2415d10a041 · 2026-04-07 · XML-reference inventory.
- S-b32e8bae0905 · 2026-11-04 · Protocol implementation and registration evidence.
- S-b4058c3be3bd · 2026-11-09 · Protocol implementation and registration evidence.
- S-bad77f83f7f6 · 2026-11-09 · Protocol implementation and registration evidence.
- S-c2596d00b100 · 2026-11-09 · Portal interfaces, direct capture and sandbox boundary.
- S-c34c6ce697a3 · 2026-11-09 · Protocol implementation and registration evidence.
- S-c4bef886de89 · 2026-11-09 · Protocol implementation and registration evidence.
- S-c4fb695b7c41 · 2026-11-09 · Protocol implementation and registration evidence.
- S-c5d3db1884f1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-d2752f2d6c22 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e0a6e2a8d6f1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e7392f226f69 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e9dc5003d109 · 2026-10-06 · Protocol implementation and registration evidence.
- S-ef6c7832a511 · 2026-11-09 · Image-copy and capture-source manager registration.
- S-fd840029644f · 2026-10-22 · Protocol implementation and registration evidence.
