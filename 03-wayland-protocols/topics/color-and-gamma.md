---
id: "protocols/topics/color-and-gamma"
title: "Per-output color and gamma"
kind: "topic"
layers: ["engine:drm", "compositor", "protocol", "sdk"]
domicile_status: "n/a"
domicile_refs: []
checked_at: "2026-11-09"
scroll_parity: "baseline"
evidence_mix: "mostly-source"
boundary_note: false
bucket: "domicile"
tags: ["layer:protocol", "evidence:source"]
sources: [S-18a2678e3352, S-3f524db52413, S-4378aebc583f, S-74f557738f56, S-8100b35ed50a, S-b824688e0e5c, S-c4e64d97971e, S-e5e4d7fe8e6a, S-f9c430a954b3]
family: protocols
---

# Per-output color and gamma

A gamma ramp changes the output hardware transfer table. Color-management protocols describe color meaning for surfaces and displays. Those are separate contracts. [docs]

## Protocols at the pin

| Protocol | Purpose | Domicile disposition |
|---|---|---|
| `wlr-gamma-control-unstable-v1` | A client sets one output's three-channel ramp | Advertised manager version 1; client and night-light ramp ownership are mutually exclusive |
| `color-management-v1` | Surface and output color descriptions | Absent in the complete executable compositor registration search |
| `color-representation-v1` | Describe alpha mode, matrix coefficients, value range and chroma location | Absent in the same registration search |

The gamma manager is instantiated directly. The table's absent entries describe registration at the pin, rather than a judgment about the rendering path. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/gamma_control.rs#L25), [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L817) [source]

Color representation controls straight versus premultiplied alpha, H.273 matrix/range selection and subsampled chroma positioning. Color management separately carries image descriptions, render intent and output/preferred feedback. [representation schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/color-representation/color-representation-v1.xml#L356), [management schema](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.49/staging/color-management/color-management-v1.xml#L730) [source]

## Landed behavior

- A night-light temperature can target every monitor or one named monitor. A gamma-control client temporarily owns the complete ramp on its monitor. On release or client exit, night light resumes. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L266) [docs]
- One owner holds a monitor at a time. Client ramps do not multiply by the night-light ramp. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L276) [docs]
- The engine uses a hardware ramp, without requiring a color-transformation matrix. The monitor's reported ramp size determines the upload shape. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L291) [docs]
- Ramps are restored after modesetting and resent after display reports, covering console return, resume, hotplug and engine replacement. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L302) [docs]
- The merged change [#1921](https://github.com/cprussin/domicile/pull/1921) settles the per-output behavior raised in [#1915](https://github.com/cprussin/domicile/issues/1915). The request is answered upstream. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L266) [docs]

## Limits at the pin

DisplayLink outputs and nested or headless runs can have no hardware ramp; the gamma client then receives protocol failure. An unplugged target does too. A nested engine leaves display color to the host. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L294) [docs]

A changed night-light value can stay invisible while a display is idle until another frame is rendered. The pinned upstream gaps record this separately from ramp ownership. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md#L39) [docs]

A working warmth or dimming ramp does not establish ICC profile support, wide-gamut application rendering or an HDR scanout path. [inferred]

## Lineage

- 2026-11-04–2026-11-05: the color studies distinguish global white-point state, per-monitor state and hardware effect, with physical application left unmeasured in those studies. S-c4e64d97971e · 2026-11-05 · Output color state and restatement study [docs]
- 2026-11-09: the pinned display contract records per-monitor ramps and exclusive client ownership. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md#L266) [docs]

Related feature: [per-output night light and gamma ramps](../../06-features/outputs/per-output-color-and-gamma.md).

## Sources

- S-b824688e0e5c · 2026-11-08 · Color-representation 1.49 alpha and YCbCr contract.

- S-18a2678e3352 · 2026-11-08 · Color-management 1.49 image-description contract.

- S-8100b35ed50a · 2026-11-09 · Pinned upstream behavior and client limits.

- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-4378aebc583f · 2026-11-06 · Projected protocol members and schema coordinates.
- S-74f557738f56 · 2026-11-09 · Output profiles, pointer transforms and gamma contract.
- S-c4e64d97971e · 2026-11-05 · Output color state and restatement study.
- S-e5e4d7fe8e6a · 2026-11-08 · Dated desktop behavior observations.
- S-f9c430a954b3 · 2026-11-09 · Per-output gamma-control manager registration.
