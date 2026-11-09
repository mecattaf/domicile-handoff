---
id: protocols/wayland-explorer-entry
title: A compositor entry on Wayland Explorer
kind: topic
layers: [compositor, protocol, external]
domicile_status: n/a
domicile_refs: []
checked_at: "2026-11-09"
scroll_parity: n/a
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [layer:protocol, evidence:source]
sources: [S-613804135659, S-ae7145121a23, S-ec33f5755481]
---

# A compositor entry on Wayland Explorer

Wayland Explorer publishes protocol documentation and a compositor support comparison. Its compositor records describe registry globals at a captured release. They do not certify every request, event or policy path. [source] [Registry schema](https://github.com/vially/wayland-explorer/blob/6af4bee21d23e57468921eb9759c3a1ae61d2dbc/src/data/compositor-registry.ts#L1)

## Record contents

The current schema stores a generation timestamp, the compositor version, and a list of global interface names with their advertised integer versions. A registry item supplies the compositor identifier, display name, optional icon and record. [source] [Schema and registry](https://github.com/vially/wayland-explorer/blob/6af4bee21d23e57468921eb9759c3a1ae61d2dbc/src/data/compositor-registry.ts#L1)

The repository documents `wlprobe` as the producer of these captures. Its sway example identifies release 1.11. That example is separate from this library's pinned sway source comparison. [source] [Capture instructions](https://github.com/vially/wayland-explorer/blob/6af4bee21d23e57468921eb9759c3a1ae61d2dbc/src/data/compositors/README.md#L1) · [Release record](https://github.com/vially/wayland-explorer/blob/6af4bee21d23e57468921eb9759c3a1ae61d2dbc/src/data/compositors/sway.json#L1)

## Domicile evidence

The [coverage map](coverage-map.md) and [global-interface table](rows/protocol-globals-domicile.tsv) record source registrations at Domicile's pin. A community-site entry additionally names a release and contains a registry capture from that release. Renderer availability, backend and client permissions can affect which globals appear; the source map preserves such conditions. [inferred]

This library describes the entry format. No entry is filed.

## Sources

- [Wayland Explorer repository](https://github.com/vially/wayland-explorer/tree/6af4bee21d23e57468921eb9759c3a1ae61d2dbc).
- S-ae7145121a23 · 2026-11-09 · Community compositor capture instructions.
- S-ec33f5755481 · 2026-11-09 · Community compositor registry schema.
- S-613804135659 · 2026-11-09 · Published sway release capture.
