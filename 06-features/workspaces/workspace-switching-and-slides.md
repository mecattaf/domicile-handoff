---
id: workspaces/workspace-switching-and-slides
title: Workspace switching and slides
kind: feature
family: workspaces
layers: [manganese, engine:blink]
domicile_status: landed
domicile_refs: [cprussin/domicile#1906, cprussin/domicile#1942, packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: true
scroll_parity: baseline
donors: [sway, scroll]
eras: []
rows: []
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: false
bucket: domicile
tags: [family:workspaces, layer:manganese, status:landed, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-23c521755f22, S-37f72d290d44, S-56622eb226ce, S-914b44f9321b, S-9fe5a8a3f191, S-c40e302fa0da, S-c9366e4f4442, S-d5b15decb594, S-fd0c87388c02]
---
# Workspace switching and slides

Workspace switching changes the shown workspace while a screen-local slide keeps the transition continuous.

## Behavior

- Switching and returning to the previous workspace are separate actions.
- Slides clip moving content to its screen, including floats near an edge.
- Incoming hidden content can use its last frame while it begins drawing again.
- A switch at an end of the workspace row preserves a continuous motion path.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| sway @ pinned source | Workspace selection, next/previous and back-and-forth | Navigation can be restricted to the current output. | [Workspace commands](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L931) |
| scroll @ pinned source | Workspace navigation around scrolling layouts | Overview and workspace selection are separate navigation mechanisms. | [Workspace interaction](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L249) |

## What the notes found

- Checked 2026-11-09: The public slide investigation records a surface-deadline wait at arrival; applying the resizing hint removes that wait in its headless check. Hardware presentation timing remains unmeasured. [source] [#1942](https://github.com/cprussin/domicile/pull/1942)
- 2026-11-09: sway move-to-workspace accepts prev_on_output, next_on_output and back_and_forth targets alongside numbers and names; workspace_auto_back_and_forth makes a repeated workspace command return to the previous workspace (sway@1652c54) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L264> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L971>
- 2026-11-09: Manganese key docs list Meta+1 to Meta+0 as workspace 1-10 switches and Meta+Shift+1 to 0 as sending the window to workspace 1-10 while staying; the workspace command returns to the previous workspace if the target is already shown [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L13> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L54>
- 2026-11-04: In a headless Domicile desktop with the stock retained layout, one fixture window moved to numbered workspace 2, took focus there and returned to the first workspace (observed on Domicile at a 2026-11-04 build) [measured]
- 2026-11-04: Mod plus wheel workspace switching has a page-level mechanism through the grab layer and useWorkspaces, per source reading; a fullscreen webview may be an exception [inferred]

## Where it touches Domicile

- `manganese`: Workspace commands select, step and return to the previous workspace. Slides clip each part to its screen and mark arriving window elements as resizing. [source] [Workspace commands](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L54) · [Slide policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L400) · [#1942](https://github.com/cprussin/domicile/pull/1942)
- `engine:blink`: An embedding element marked resizing shows its latest available client or browser frame immediately at the frame’s own size, rather than waiting for a newly sized frame. [source] [Native resize embedding](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/docs/ELEMENTS.md#L17) · [Browser resize embedding](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/docs/ELEMENTS.md#L83)
- Status: `landed`; the row-end correction in [#1906](https://github.com/cprussin/domicile/pull/1906) and arrival fix in [#1942](https://github.com/cprussin/domicile/pull/1942) are merged and reflected in the pin. [source]

## Lineage

- 2026-11-07: workspace row-end behavior was corrected. [source] [#1906](https://github.com/cprussin/domicile/pull/1906)
- 2026-11-08: the slide-start wait was removed for incoming hidden pages. [source] [#1942](https://github.com/cprussin/domicile/pull/1942)

## Sources

- S-9fe5a8a3f191 · 2026-09-21 · [Container commands and compositor configuration](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L1).
- S-37f72d290d44 · 2026-11-05 · [Scrolling viewport and navigation semantics](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L1).
- S-23c521755f22 · 2026-11-09 · [Grouped layouts, workspace homes and interaction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L1).
- S-d5b15decb594 · 2026-11-09 · [Window command vocabulary and default bindings](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L1).
- S-fd0c87388c02 · 2026-11-08 · Public Domicile change catalog.
- S-914b44f9321b · 2026-11-09 · [Native and browser embedding element behavior](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/docs/ELEMENTS.md#L1).
