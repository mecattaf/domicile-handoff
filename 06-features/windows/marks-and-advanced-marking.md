---
id: "windows/marks-and-advanced-marking"
title: "Window marks, selections and trails"
kind: "feature"
family: "windows"
layers: ["manganese", "any-shell", "sdk"]
domicile_status: "absent"
domicile_refs: ["packages/manganese-window-management/docs/KEYS.md"]
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
answered_upstream: false
scroll_parity: "baseline"
donors: ["sway", "scroll"]
eras: []
rows: ["SW-0156", "SW-0184", "SW-0222", "SW-0223", "SW-0232", "SW-0240", "SW-0241", "SC-0039", "SC-0040", "SC-0041", "SC-0042", "SC-0043", "SC-0067", "SC-0072", "SC-0073", "SC-0076", "SC-0081", "SC-0083", "SC-0086", "SC-0088", "SC-0089", "SC-0201", "SW-0484", "SW-0485", "SW-0545", "SW-0546", "SW-0547", "SW-0618", "SW-0619", "SW-0620", "SW-0621", "SW-0622", "SW-0623", "SC-0234", "SC-0235", "SC-0236", "SC-0237", "SC-0238", "SC-0239", "SC-0240", "SC-0241", "SC-0242", "SC-0267", "SC-0268", "SC-0269", "SC-0270", "SC-0271", "SC-0272", "SC-0273", "SC-0274", "SC-0275", "SC-0282", "SC-0283", "SC-0299", "SC-0300", "SC-0301", "SC-0302", "SC-0303", "SC-0304", "SC-0305", "SC-0318", "SC-0319", "SC-0320", "SC-0321", "SC-0322", "SC-0327", "SC-0328", "SC-0329", "SC-0330", "SC-0331", "SC-0332", "SC-0333", "SC-0334", "SC-0335", "SC-0336", "SC-0337", "SC-0460", "SC-0501", "SC-0502", "SC-0503", "SC-0504", "SC-0505", "SC-0513", "SC-0528", "SC-0529", "SC-0530", "SC-0531", "SC-0539", "SC-0540", "SC-0541", "SW-1142", "SC-0611", "SC-0693", "SC-0704", "SC-0705"]
depends_on: []
evidence_mix: "mostly-source"
reported_on_domicile: []
boundary_note: false
bucket: "domicile"
tags: ["family:windows", "layer:manganese", "status:absent", "evidence:source", "scroll-parity:baseline", "bucket:domicile"]
sources: ["S-9fe5a8a3f191", "S-204a969601d0", "S-d5b15decb594", "S-23c521755f22", "S-e77ffff40807", "S-017f66cda9c3", "S-16174e406a0b", "S-d1e267110753", "S-759448a412d0"]
---

# Window marks, selections and trails

Marks name containers; selections and trails add multi-window operations and ordered return paths.

## Behavior

- A named mark identifies one container; add, replace, toggle and remove have separate meanings.
- A selection can contain windows across workspaces and can move as a set.
- A trail orders anonymous marks and supports stepping through frequently revisited windows.
- Jump labels offer a temporary target picker; target kind and workspace scope are separate arguments.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| sway @ pinned source | Unique container marks and mark criteria | A mark can target focus, move and swap operations. | [Marks](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L826) |
| scroll @ pinned source | Named marks, selection, trails and trailmarks | Selections can span workspaces; trails provide ordered anonymous marks. | [Selections](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L890) · [Trails](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1109) |

## What the notes found

- Checked 2026-11-09: Scroll’s jump command separates the target kind from the final active/all workspace scope. A target kind of `all` alone does not establish all-workspace scope. [source] [Jump grammar](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L596)
- Checked 2026-11-09: Sway’s marks can label non-view groups. Literal lookup traverses output containers, hidden scratchpad trees and saved fallback workspaces; the con_mark criterion instead uses a regular expression. Command targeting and per-view rule matching have different scopes. [source] [Mark command](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/mark.c#L20) · [Literal lookup](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/root.c#L292) · [Command collector](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/criteria.c#L500)

## Where it touches Domicile

- `manganese`: marks, selection trails and their command vocabulary are not addressed as of the pin. The bounded absence search covers the complete command parser, window-state declarations and documented command inventory. [source] [Command inventory](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L38) · [Differences](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L160)
- `sdk`: The window namespace supplies live client identities and change events. [source] [Window state and events](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L138)
- Status: `absent` for the donor marking contract; this disposition records the searched command and state surfaces. [source] [Layout state](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L6)

## Lineage

- 2026-11-05: the pinned Scroll vocabulary includes named marks, selections, jump labels and trails as distinct facilities. [source] [Marking commands](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L890)

## Sources

- S-9fe5a8a3f191 · 2026-09-21 · [Container commands and compositor configuration](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L1).
- S-204a969601d0 · 2026-11-06 · sway and Scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · [Window command vocabulary and default bindings](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L1).
- S-23c521755f22 · 2026-11-09 · [Grouped layouts, workspace homes and interaction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L1).
- S-e77ffff40807 · 2026-11-09 · [Shell entry, client elements and native-window control](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L1).
- S-017f66cda9c3 · 2026-09-21 · [Container marks and replacement modes](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/mark.c#L1).
- S-16174e406a0b · 2026-09-21 · [Container lifetime, marks and ownership](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/container.c#L1).
- S-d1e267110753 · 2026-09-21 · [Root traversal and literal mark lookup](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/root.c#L1).
- S-759448a412d0 · 2026-09-21 · [Command selectors and per-view matching](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/criteria.c#L1).
