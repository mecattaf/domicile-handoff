---
id: sway-family/scroll-parity/virtual-outputs
title: Virtual outputs
kind: family
family: workspaces
layers: [any-shell, compositor]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-04be5de691f5, S-0d9c2ee30def, S-204a969601d0, S-23c521755f22, S-4a2b900f6a8d, S-9ee716b1ba9a, S-d5b15decb594, S-f278a9071404]
---

# Virtual outputs

12 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 0 same; 1 scroll-changed; 11 scroll-only; 0 sway-only.
- Domicile coverage: 0 landed; 0 partial; 12 absent; 0 unverified; 0 accounting aliases/exclusions.
- Mechanism layers: any-shell 12; compositor 12.
- Feature: [workspaces/workspace-model-and-homes](../../06-features/workspaces/workspace-model-and-homes.md).

## Donor semantics

`virtual_output split` subdivides the region containing the focused workspace. Each child owns its workspace list and layout. Splits can nest, retaining a chosen fraction and an explicit gap. Reset removes the containing split and gathers the removed regions' workspaces into the survivor. [source] [Region contract](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1787).

A maximize toggle expands one region over the physical output and hides its peers. Selecting a workspace in another region changes which region occupies the physical display. Directional output focus tries regions before other physical outputs. [source] [Maximize](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1800) · [Focus](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L524).

This does not create a DRM connector or a headless monitor. Domicile's `addVirtual` creates a separate connectorless monitor whose content is available to capture. Its existence does not establish recursive region parity. [source] [Domicile virtual monitor](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/displays.ts#L21).

Resetting a split keeps the selected leaf, moves workspaces from the removed subtree into it, and removes empty workspaces that are not focused by the current seat. Maximizing gives the active leaf the physical output area while retaining the split rectangles used for directional navigation; region visibility follows the active workspace. Pointer lookup falls back to the active region when no visible region contains the point. [source] [Reset and merge](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/virtual_output.c#L457) · [Saved rectangles](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/virtual_output.c#L274) · [Region navigation and hit testing](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/virtual_output.c#L535).
The parser defaults to a vertical half split with no gap. Fractions are clamped to the 0.1–0.9 range and negative gaps become zero. Directional aliases choose an axis; the existing leaf keeps the first portion and the new leaf receives the second. Maximization accepts either a toggle or an idempotent on/off value. Startup/no-output and validation cases defer the operation before changing the live tree. [source] [Parser and target context](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/virtual_output.c#L21) · [Axis and limits](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/virtual_output.c#L80).


Region workspace lists sort numeric-leading names numerically before other names, using stable ordering for equal prefixes and nonnumeric names. Attachment selects an active member only when none exists; detaching the active workspace chooses the first remaining member. Region reassignment can first move a workspace to the target physical output. Output destruction clears workspace region references and tiling clip-box pointers before freeing the regions. [source] [List order and attachment](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/virtual_output.c#L296) · [Detach and reassignment](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/virtual_output.c#L341) · [Output teardown](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/virtual_output.c#L70).

## Lineage

- 2026-11-05: the donor changes to recursive `virtual_output` regions and removes the earlier `workspace split` command. Older material using that spelling belongs to the previous donor state. [source] [Pinned change](https://github.com/dawsers/scroll/commit/b6586d273fed7d187744b7eaec2c234386a029fa).
## At the pin

Not addressed at the pin: the current command and display namespaces expose connectorless virtual monitors, rather than recursive regions of one physical output. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L144) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/displays.ts#L21).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-04be5de691f5 · 2026-11-05 · Scroll reference at the donor revision.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-9ee716b1ba9a · 2026-11-05 · Scroll reference at the donor revision.
- S-f278a9071404 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
