---
id: sway-family/any1-branch
title: Any1 branch investigations
kind: collection
layers: [external, any-shell]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: beyond
donors: [sway]
evidence_mix: mostly-source
bucket: domicile
tags: [bucket:domicile, evidence:source, scroll-parity:beyond]
sources: [S-364ab97df118, S-63bf9b357e57, S-ed24ede20694, S-33141ff17c34, S-78a98efb6748, S-9d812ac89cf4, S-50ea8a1ddf1e, S-2b2b4280d993, S-50c123e64a11, S-ac32187c6a0c]
---

# Any1 branch investigations

The Any1 branch catalog mixes upstreamed device and seat work with experimental focus metadata and per-container state. The public source reference here is the historical `seat-features` revision dated 2023-04-29, examined by the 2026 studies. [docs] [Historical revision](https://github.com/any1/sway/commit/9d60831231375110a7d1b03d52a5c7df674b40bc)

## Distinct contracts

| Contract | Evidence and limit |
|---|---|
| Several seats focused on a node | The branch serializes `focused_seats` by comparing each seat's focus with the node. [source] [node metadata](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/ipc-json.c#L764) |
| Most recent seat activity | Container JSON carries `most_recent_seat` when that state exists; this differs from the default seat's `focused` field. [source] [container metadata](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/ipc-json.c#L809) |
| Per-container rule state | Local state overrides the parent container, then the global fallback. `global` bypasses container context; `default` clears the local override. [source] [Command scope](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/commands/client.c#L82) · [Inheritance](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/tree/container.c#L1981) |
| Independent seat and cursor configuration | The branch catalog traced device attachment, transient seats and cursor behavior separately from decoration state. [docs] S-364ab97df118 · [sway seat contract](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-input.5.scd#L223) |
| Frame and damage observations | The catalog recorded a damage-stream branch as historical investigation rather than a current Domicile implementation. [docs] S-364ab97df118 |

## Override scope

The historical branch selects a container-local update whenever the command has container context, unless `global` is explicit. Resetting a local class with `default` frees that override and restores inheritance. While configuration is active, local updates rebuild the container and its descendants; the special focused-tab-title command continues through the global handler. [source] [Context dispatch](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/commands/client.c#L129) · [Reset and descendant invalidation](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/commands/client.c#L82) · [Focused-tab-title scope](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/commands/client.c#L193)

## Focus metadata lifetime

`focused_seats` tests exact node equality against each seat's current logical focus; it does not enumerate focus held by descendants. `most_recent_seat` is optional container state. Key events and pointer-frame events assign their seat to its logically focused container. [source] [Focused-seat serialization](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/ipc-json.c#L764) · [Logical-focus accessor](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/input/seat.c#L1528) · [Key-event assignment](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/input/keyboard.c#L400) · [Pointer-frame assignment](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/input/cursor.c#L470)

Changing that association emits a window-focus event. Repeated activity from the same seat keeps the association without another update, and destroying that seat clears it. The field contains a seat name, without an activity timestamp. These source contracts do not establish observed multi-seat behavior in the recorded build. [source] [Association and cleanup](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/tree/container.c#L2013) · [IPC value](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/ipc-json.c#L809)

## What the notes found

- A 2026-10-22 study compiled a carried-forward patch series but did not run that compositor. Its statements about runtime focus or per-container presentation remain unobserved. [docs] S-63bf9b357e57
- Per-container rule values, the seat holding keyboard focus, and the seat with the latest activity were separate state dimensions. [source] [focus fields](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/ipc-json.c#L764) · [docs] S-63bf9b357e57
- The 2026-10-23 catalog distinguished older device and transient-seat work already represented in sway from additional branch behavior. These are historical dispositions, not fresh statements about every remote branch. [docs] S-364ab97df118

## Domicile boundary

Domicile's [keybinding architecture](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/KEYBINDINGS.md) and [shell configuration](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-CONFIG.md) define its current public surfaces. The Any1 findings add donor contracts to compare; they do not establish a Domicile status by themselves. [docs]

For related behavior, see [window criteria](../06-features/windows/criteria-and-window-rules.md), [binding modes](../06-features/bindings-and-modes/binding-modes.md) and the [sway-next record](sway-next.md).

## Lineage

- 2026-10-22: source investigation separated focus metadata from per-container rule state. [docs] S-63bf9b357e57
- 2026-10-23: the branch catalog recorded which mechanisms had upstream equivalents and which remained additional contracts. [docs] S-364ab97df118

## Sources

- S-ed24ede20694 · 2023-04-29 · [Historical Any1 focus metadata at the historical revision](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/ipc-json.c#L764).
- [Per-window state discussion](https://github.com/swaywm/sway/issues/3177)
- [Transient-seat history](https://github.com/swaywm/sway/pull/5921)
- S-364ab97df118 · 2026-10-23 · Historical branch and capability catalog
- S-63bf9b357e57 · 2026-10-22 · Per-container state and focus metadata study

- S-78a98efb6748 · 2023-04-29 · [Historical Any1 state inheritance and seat association at the historical revision](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/tree/container.c#L2013).
- S-9d812ac89cf4 · 2023-04-29 · [Historical Any1 keyboard attribution at the historical revision](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/input/keyboard.c#L400).
- S-50ea8a1ddf1e · 2023-04-29 · [Historical Any1 pointer-frame attribution at the historical revision](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/input/cursor.c#L470).

- S-50c123e64a11 · 2023-04-29 · [Historical Any1 override scope and reset at the historical revision](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/commands/client.c#L82).
- [Historical parent and global inheritance](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/tree/container.c#L1981)

- S-33141ff17c34 · 2023-04-29 · [Historical Any1 revision metadata](https://github.com/any1/sway/commit/9d60831231375110a7d1b03d52a5c7df674b40bc).
- S-2b2b4280d993 · 2023-04-29 · [Historical Any1 logical-focus lookup at the historical revision](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/sway/input/seat.c).
- S-ac32187c6a0c · 2023-04-29 · [Historical Any1 container-state declarations at the historical revision](https://github.com/any1/sway/blob/9d60831231375110a7d1b03d52a5c7df674b40bc/include/sway/tree/container.h).
