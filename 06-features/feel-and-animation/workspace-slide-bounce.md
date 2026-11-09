---
id: feel-and-animation/workspace-slide-bounce
title: Workspace slide and bounce continuity
kind: feature
family: feel-and-animation
layers: [manganese, engine:blink]
domicile_status: landed
domicile_refs: [cprussin/domicile#1887, cprussin/domicile#1906, cprussin/domicile#1942]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: true
scroll_parity: baseline
donors: [scroll]
eras: []
rows: []
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: false
bucket: domicile
tags: [family:feel-and-animation, layer:manganese, status:landed, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-09fd0ca98c14, S-204a969601d0, S-20e287046b4a, S-23c521755f22, S-28be5e927839, S-76a2d624f255, S-914b44f9321b, S-fd0c87388c02]
---
# Workspace slide and bounce continuity

Workspace transitions preserve a continuous slide across repeated switches and keep the switcher’s feedback aligned with the selected workspace.

## Behavior

- A repeated switch can interrupt an existing slide without returning to an old starting point.
- Cross-screen clipping follows the screen whose workspace changes.
- The switcher chip can overshoot and settle while the workspace slide keeps its own motion.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| scroll @ pinned source | Interruptible animation variables | Retargeting begins at the displayed value for each affected variable. | [Animation state](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/include/sway/desktop/animation.h#L53) |

## What the notes found

- Checked 2026-11-09: The workspace chip bounce, row-end continuity and incoming-page wait are distinct public slices; a single animation label does not account for all three. [source] [#1887](https://github.com/cprussin/domicile/pull/1887) · [#1906](https://github.com/cprussin/domicile/pull/1906) · [#1942](https://github.com/cprussin/domicile/pull/1942)
- 2026-11-09: scroll animation config takes a per-operation entry (default, window_move, window_move_float and others) with enabled flag, duration and a curve; unset entries fall back to default, which is on at 300 ms with a cubic curve (scroll@b6586d2) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L84-L92>
- 2026-11-06: A workspace slide kept guest, native windows and PTYs alive through a config refresh in lifecycle tests on 2026-11-05 and 2026-11-06 [measured]
- 2026-11-08: Arriving windows mark their app or webview as resizing, displaying the latest available frame without waiting for a fresh embedded-page frame at slide start. The public change leaves physical GPU timing unqualified [docs] <https://github.com/cprussin/domicile/pull/1942>
- 2026-11-06: Manganese slides workspaces horizontally over 400 ms by default; timing and direction are motion settings, and a global reduced-motion rule overrides a configured duration [docs]

## Where it touches Domicile

- `manganese`: The workspace chip has a bounce transition, while workspace slides remain continuous at row boundaries and clip to the active screen. Arriving window elements receive the resizing hint. [source] [#1887](https://github.com/cprussin/domicile/pull/1887) · [#1906](https://github.com/cprussin/domicile/pull/1906) · [Slide policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L400) · [#1942](https://github.com/cprussin/domicile/pull/1942)
- `engine:blink`: The resizing embedding contract shows the latest available native or browser frame immediately at its own size during the slide. [source] [Native resize embedding](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/docs/ELEMENTS.md#L17) · [Browser resize embedding](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/docs/ELEMENTS.md#L83)
- Status: `landed`; each cited slice is merged before the pin. Hardware presentation timing is not established by the headless slide check. [source] [#1942](https://github.com/cprussin/domicile/pull/1942)

## Lineage

- 2026-11-06: the switcher chip acquired bounce feedback. [source] [#1887](https://github.com/cprussin/domicile/pull/1887)
- 2026-11-07: row-end workspace motion was corrected. [source] [#1906](https://github.com/cprussin/domicile/pull/1906)
- 2026-11-08: incoming hidden pages no longer delay slide start in the documented headless check. [source] [#1942](https://github.com/cprussin/domicile/pull/1942)

## Sources

- S-204a969601d0 · 2026-11-06 · sway and Scroll semantic inventory.
- S-23c521755f22 · 2026-11-09 · [Grouped layouts, workspace homes and interaction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L1).
- S-fd0c87388c02 · 2026-11-08 · Public Domicile change catalog.
- S-914b44f9321b · 2026-11-09 · [Native and browser embedding element behavior](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/docs/ELEMENTS.md#L1).
