---
id: shell-surfaces/layer-shell-equivalents
title: Panels and overlays in one desktop page
kind: feature
family: shell-surfaces
layers: [engine:blink, compositor, sdk, any-shell]
domicile_status: by-design-elsewhere
domicile_refs: [docs/architecture/ONE-PAGE-FOR-THE-DESK.md, docs/architecture/WINDOW-COMPOSITING.md]
checked_at: "2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc"
answered_upstream: false
scroll_parity: n/a
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:shell-surfaces, status:by-design-elsewhere, bucket:domicile, evidence:source]
sources: [S-1009b314f694, S-17b9826676ac, S-1cd4484a3a81, S-3c995b7a34f5, S-45ec4e7fb9f9, S-4ed3510bbb79, S-514bff6c69a0, S-526eea98076e, S-703c0d408b29, S-71e61bbecc5d, S-7c703de4883d, S-80c940f0be28, S-85883d471d8c, S-9c59f8ebc0df, S-9e14c851272d, S-a4a45fc9bd39, S-a514b4f6d553, S-a542cf9c1edb, S-c40e302fa0da, S-c82d3c1ba004, S-ca5733e7be78, S-e4ff18df1f16]
donors: [views, scroll]
eras: [quickshellX / webshell, COWL, views-shell]
rows: [AL-exo-core-039, AL-exo-wayland-044, AL-views-shell-vtabs-001, AL-views-shell-vtabs-003, AL-views-shell-vtabs-004, AL-views-shell-vtabs-005, AL-views-shell-vtabs-006, AL-views-shell-vtabs-007, AL-views-shell-vtabs-009, AL-views-shell-vtabs-010, AL-views-inventory-089, AL-cade-triage-081, AL-cade-decision-068, AL-cade-decision-101, AL-cade-machinery-wm-006, AL-cade-spec-coverage-009, AL-cade-niri-coverage-059, AL-cade-audit-tables-003, AL-april-protocol-triage-024, AL-april-protocol-triage-040, AL-ash-shell-inventory-001, AL-ash-shell-inventory-011, AL-rt-tree-findings-046, AL-window-tree-spec-001, AL-window-tree-spec-002, AL-window-tree-spec-003, AL-window-tree-spec-004, AL-window-tree-spec-005, AL-window-tree-spec-006, AL-window-tree-spec-007, AL-window-tree-spec-008, AL-window-tree-spec-009, AL-surface-matrix-001, AL-surface-matrix-002, AL-surface-matrix-003, AL-surface-matrix-004, AL-surface-matrix-005, AL-surface-matrix-006, AL-surface-matrix-007, AL-surface-matrix-008, AL-surface-matrix-010, AL-surface-matrix-011, AL-surface-matrix-013, AL-surface-matrix-014, AL-surface-matrix-015, AL-surface-matrix-016, AL-surface-matrix-017, AL-surface-matrix-018, AL-surface-matrix-019, AL-surface-matrix-020, AL-surface-matrix-021, AL-surface-matrix-022, AL-surface-matrix-023, AL-surface-matrix-035, AL-surface-matrix-036, AL-surface-matrix-037, AL-engine-check-003]
depends_on: []
reported_on_domicile: []
---
# Panels and overlays in one desktop page

A shell places panels and overlays within the desktop page. Native client placement and scene composition use that page's elements rather than requiring a separate shell surface for every panel. [One desktop page](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md#L3). [docs]

## Behavior

- A shell element can span a monitor seam and share one scene clock.
- Reserved layout space remains a shell-layout contract; it is distinct from accepting an external layer-shell client.
- An external client can attach an anchor name to its toplevel; a matching shell places its `<app>` at that anchor. [Client anchors](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L112). [docs]

## How donors do it

| Donor @ pin | Mechanism | Boundary | Link |
|---|---|---|---|
| Chromium Views @ 150 | Widget and Linux window-tree host | Native surface role belongs to the platform host | [Linux host](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ui/views/widget/desktop_aura/desktop_window_tree_host_linux.h) |
| Domicile @ checked pin | One page, multiple presentation windows | Page layout owns shell geometry; presenters scan out regions | [Presenter contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md#L46) |

- In the pinned [Scroll donor](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L1620), layer-popup buffers scale during animation while their positions are adjusted separately. Parent desired-size fields and relative popup position determine edge anchoring or centering. [source] This external-compositor path supplies a geometry comparison alongside page-owned panels. [inferred]

## What the notes found

- The client-shell census treats shell surface creation, popup membership and host authority as separate seams. S-17b9826676ac. [docs]
- This comparison concerns the behavior of shell-owned panels. It does not establish external `wlr-layer-shell` protocol coverage. [inferred]
- 2026-11-09: Domicile advertises domicile_anchor_manager_v1 version 1; a native toplevel names a page anchor, with anchor state committed alongside surface state (Domicile main pin) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/anchors.rs#L35> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/anchors.rs#L44>
- 2026-10-23: A historical integration audit distinguished compositor-resident dock and live overview machinery from separate panel and notification clients; replacing a visible process did not transfer compositor-owned geometry and hit-testing (otto@v1.8.0) [docs] <https://github.com/nongio/otto/blob/v1.8.0/src/workspaces/window_selector.rs> <https://github.com/nongio/otto/blob/v1.8.0/components/otto-bar/src/app.rs>
- 2026-10-23: A native-host reuse audit isolated mutation batching and retained-node bookkeeping from backend-specific focus, editor, layout, painting and platform-window lifetime; a shared React authoring surface did not transfer those native behaviors (GPUIX@a53b294401280014e94eb9bd98a7c7ed1929f92d) [docs] <https://github.com/remorses/gpuix/blob/a53b294401280014e94eb9bd98a7c7ed1929f92d/packages/native/js/mutations.ts> <https://github.com/remorses/gpuix/blob/a53b294401280014e94eb9bd98a7c7ed1929f92d/packages/native/src/renderer.rs>
- 2026-11-02: A native layer-window wrapper exposes keyboard exclusivity and reserved desktop space independently; alpha-transparent pixels still need an explicit input region for click-through. This is a source-linked historical native-host survey, not a Domicile runtime result (Astal4@cbcd9f49dd6b9638dc5623b56cc6e1e0a60b593e) [docs] <https://github.com/Aylur/astal/blob/cbcd9f49dd6b9638dc5623b56cc6e1e0a60b593e/lib/astal/gtk4/src/widget/window.vala> <https://github.com/wmww/gtk4-layer-shell/blob/69ad9a02a8c31ac2d3db6710f43b3d8434559e46/include/gtk4-layer-shell.h>
- 2026-10-06: A 2026-10-06 comparison distinguishes application protocol bindings from native shell protocol clients; stock Ozone coverage does not establish layer-shell, lock, workspace-list or capture-client integration (Chromium 154/Qt6.11) [docs] <https://github.com/chromium/chromium/blob/154.0.8037.92/ui/ozone/platform/wayland/host/wayland_connection.cc#L153>
- 2026-07-16: A 2026-07-16 catalog separates native panel protocol placement from direct placement of compositor-owned shell surfaces (Wayland) [docs]
- 2026-11-02: A workspace sidebar spike (2026-10-26..11-03) separated reserved-space layer surfaces from ordinary preview windows; the renderer process lifetime determined which local state survived hiding (sway@1.12) [docs]
- 2026-10-06: The Otto layer-surface study separated configured output, drawn container and hit-test output; top and overlay containers were primary-output scenes while input retained the requested output (otto@v1.6.0) [docs]
- 2026-10-26: The native panel study treated keyboard interactivity, pointer input region and exclusive space as separate state; destroying a layer surface releases its zone while retaining only application state held outside the view (sway@1.12) [docs]
- 2026-06-27: The bar study separates visible state, reserved workspace area and input region; auto-hide keeps a reveal strip active and suppresses hiding while an attached menu is open (noctalia@2026-06-27-study) [docs]
- 2026-11-04: Per-device touch routing, kanshi-style profiles and layer-shell were classed in the notes as covered at the pin: input.pointers and output.profiles cover the first two and panels are page layout [inferred]
- 2026-11-04: A 2026-11-04 source reading reported no wlr-layer-shell support; the shell draws panels as DOM elements, and desktop roles are meant to live in shell modules [docs]

## Where it touches Domicile

- `engine:blink` and `any-shell`: page layout supplies the panel and overlay scene.
- `compositor` and `sdk`: client anchors give a native window a shell-defined placement name; an unknown anchor leaves ordinary window placement. [Anchor semantics](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L112). [docs]
- Status at the pinned checkout: `by-design-elsewhere` for shell-owned layer surfaces, assigned to the page by [the one-page design](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md#L3). External protocol coverage belongs in the protocol map. [docs]

- Native anchor names are [double-buffered with surface commits](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/anchors.rs#L105). The [factory rejects a second live anchor object](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/anchors.rs#L148) for the same toplevel. [Destroying it clears the pending name](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/anchors.rs#L155) and permits replacement while the surface survives. [source]

## Lineage

- quickshellX and COWL used separate native shell surfaces under niri.
- The [ownership mechanism](../../01-chromium-lineage/mechanisms/layer-shell-client-and-compositor.md) retains their external-compositor boundary.

## Sources

- S-17b9826676ac · 2026-10-01 · Chromium window-system and shell seam census
- S-e4ff18df1f16 · 2026-11-09 · Pinned one-page desktop design
- S-514bff6c69a0 · 2026-11-09 · Pinned engine surface architecture
- S-3c995b7a34f5 · 2026-11-09 · Domicile native anchor object and commit lifetime
- S-703c0d408b29 · 2026-11-09 · Pinned native-window scene embedding and client anchors
- S-a4a45fc9bd39 · 2026-11-09 pin check · Chromium 150 Linux window-tree host interface
- S-9c59f8ebc0df · 2026-11-09 pin check · Scroll layer-popup animation, scaling and anchor geometry
