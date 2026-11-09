---
id: "protocols/topics/capture-and-screencopy"
title: "Capture protocols and portal capture"
kind: "topic"
layers: ["engine:viz", "compositor", "protocol", "sdk", "external"]
domicile_status: "landed"
domicile_refs: ["cprussin/domicile#1107", "cprussin/domicile#1510"]
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: "baseline"
evidence_mix: "mostly-source"
boundary_note: true
bucket: "domicile"
tags: ["layer:protocol", "evidence:source"]
sources: [S-24f31085af99, S-5fb9706b471e, S-c2596d00b100, S-c34c6ce697a3, S-c5d3db1884f1, S-ef6c7832a511]
family: protocols
---

# Capture protocols and portal capture

Portals mediate a user's source selection. Direct Wayland capture protocols transfer frames from an existing output or native window. Domicile serves both through its casting path. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L499) [docs]

## Interfaces at the pin

| Surface | Source scope | Permission and connection boundary |
|---|---|---|
| ScreenCast portal | Monitors, regions, native windows and browser windows, as PipeWire streams | Portal request and source picker |
| Screenshot portal | One frame of those sources | Portal access and source selection |
| wlr screencopy v3 | Output or clipped output region | Hidden from sandboxed clients; unsandboxed clients are not prompted |
| ext image-copy capture v1 | Output or ext foreign-toplevel source | The same direct-client sandbox boundary |
| ext foreign-toplevel list v1 | Native Wayland windows | Browser windows are outside this list |

The direct manager versions come from registration, and the source and permission scope comes from the pinned portal contract. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/wlr.rs#L35), [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/mod.rs#L59), [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L501) [source] [docs]

## Frame behavior

- Ext capture and wlr `copy_with_damage` wait for source damage not yet delivered to the client. The initial frame is owed in full. Ordinary wlr `copy` explicitly requests a frame even when the source has not changed. [source] [Copy request](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/wlr.rs#L250) · [Initial and undelivered damage](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/casting/streams.rs#L749) · [Forced frame request](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/casting/streams.rs#L786)
- The requested pointer can be embedded. Cursor sessions track an upright pointer image and source-relative hotspot coordinates. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L518) [docs]
- Resizing a monitor replaces its constraints and rejects the in-flight frame with a buffer-constraints reason. Removing the output stops its capture sessions. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L525) [docs]
- Capture may continue while locked. The upstream contract describes lock-screen imagery; the lock-time pixels of an isolated native-window source remain unverified. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L527) [docs]

## Lineage

- 2026-10-10: [Direct Wayland capture](https://github.com/cprussin/domicile/pull/1107) merges upstream. [docs]
- 2026-10-24: [Native-window sources and pointer sessions](https://github.com/cprussin/domicile/pull/1510) merge upstream. [docs]

Typical clients: screenshot and recording tools, remote desktop, clipboard managers, accessibility and test automation.

Related features: [screen and window capture](../../06-features/capture/screencopy-and-image-capture.md), [foreign toplevel lists](../../06-features/capture/foreign-toplevel-and-taskbar.md), [portals and sandboxed clients](../../06-features/session/portals-and-sandboxing.md).

## Sources

- S-c5d3db1884f1 · 2026-10-24 · Window-source and pointer-capture merge record.

- S-c34c6ce697a3 · 2026-10-10 · Direct Wayland capture merge record.

- S-24f31085af99 · 2026-11-09 · Capture stream damage and forced-frame scheduling.

- S-5fb9706b471e · 2026-11-09 · Wlr screencopy version registration.
- S-c2596d00b100 · 2026-11-09 · Portal interfaces, direct capture and sandbox boundary.
- S-ef6c7832a511 · 2026-11-09 · Image-copy and capture-source manager registration.
