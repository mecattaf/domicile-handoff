---
id: sway-family/sway-next
title: Sway-next: the recorded compositor fork
kind: era
layers: [external, any-shell]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: beyond
donors: [sway, scroll, swayfx]
eras: [sway-next]
evidence_mix: mostly-source
bucket: domicile
tags: [bucket:domicile, donor:scroll, donor:swayfx, era:sway-next, evidence:source]
sources: [S-12bee06598dc, S-fe928686b401, S-abb5938fade4, S-5e377e6e9b90, S-03db46ad9d28, S-aecf1c64f5c9]
---

# Sway-next: the recorded compositor fork

Sway-next combined scroll's layout with sway-compatible names and commands, swayfx-style effects, and additional window and seat state. [source] S-abb5938fade4

## Scope and dates

- 2026-10-22: the definition selected scroll as the base, retaining its scrolling layout, overview, jump, Lua surface and IPC additions. [source] S-12bee06598dc
- 2026-10-22..23: The preserved feature catalog distinguishes inherited, implemented, partial and planned items. [source] S-5e377e6e9b90
- 2026-11-08: the catalog recorded the stopping state; later planned stages had not run. [source] S-abb5938fade4

> Era and its assumptions: sway-next, which assumed layout, effects and configuration compatibility lived in one compositor fork. For Domicile this probably adds little: Domicile already exposes client windows to its web shell. Recorded because command compatibility and rendered behavior proved to be separate contracts. [docs] [Domicile shell model](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md)

## What the recorded tree contained

| Area | Technical state at the stop | Evidence |
|---|---|---|
| Scroll semantics | Scrolling layout, overview, jump, saved arrangements, trails, marks, scaling and animation behavior were inherited. | [source] S-abb5938fade4 |
| Naming and commands | Sway command names and socket aliases were retained. Split commands mapped to scroll's orientation controls. | [source] S-abb5938fade4 |
| Tabbed columns | Model, arrangement and commands existed. Software-renderer screenshots covered tabbed and stacked strip cells; pointer interaction, dragging and cycling remained incomplete or unqualified. The capability flag stayed false. | [source] S-abb5938fade4 · S-aecf1c64f5c9 |
| Effects | GLES2 blur code, cache and configuration were present. Rounded corners, shadows and dim controls mapped onto scroll's decoration machinery. | [source] S-abb5938fade4 |
| Per-window state | Container and workspace color rules included IPC readback. | [source] S-abb5938fade4 |
| Scriptability | The Lua surface retained scroll compatibility and added access to saved arrangements and activation tokens. | [source] S-abb5938fade4 |
| Seats | Per-seat command context, focus attribution, device attachment and modal ownership were recorded. | [source] S-abb5938fade4 |

## What the notes established

- A compositor rename affects socket discovery, configuration search, desktop identity and client expectations together. Preserving a command's spelling does not preserve its layout meaning. [source] S-03db46ad9d28
- The effect port involved renderer operations, scene nodes, damage expansion and container wiring; linking an alternate scene library was not equivalent to adding a blur command. [source] S-03db46ad9d28
- Software-renderer validation covered parsing and scene bookkeeping. It did not establish GLES2 blur pixels, damage correctness, cache behavior or frame cost on a GPU. No physical-desktop rendering was recorded. [source] S-5e377e6e9b90
- A frame-commit event and an event marking the end of a resize were distinct semantics in the issue catalog. [source] S-abb5938fade4
- Saved arrangements did not retain a column's tabbed or stacked mode when the study stopped. [source] S-aecf1c64f5c9

## Ending and carry-forward

Work stopped for reasons outside this library's scope; the technical state at that point was a software-renderer-qualified compositor fork with incomplete tabbed-column interaction and unverified GPU effects. [source] S-5e377e6e9b90

The contracts are recorded in the [scroll map](scroll-parity/README.md), [tiling model](../06-features/layout/tiling-splits-and-containers.md), [criteria and rules](../06-features/windows/criteria-and-window-rules.md), [overview](../06-features/workspaces/overview.md) and [Lua and shell scriptability](../06-features/ipc-scripting/lua-and-shell-scriptability.md). Seat and renderer mechanisms retain their separate layer boundaries. [inferred]

## Sources

- S-12bee06598dc · 2026-10-22 · Compositor fork definition and compatibility boundaries
- S-fe928686b401 · 2026-10-22 · Compositor state and input investigation
- S-03db46ad9d28 · 2026-10-23 · Renderer port and command compatibility plan
- S-abb5938fade4 · 2026-11-08 · Recorded compositor feature state
- S-5e377e6e9b90 · 2026-11-08 · Preserved compositor state and ending
- S-aecf1c64f5c9 · 2026-10-23 · Compositor validation limits and partial column state
