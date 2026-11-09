---
id: sway-family/scroll-parity/client-and-protocol-contracts
title: Client and protocol contracts
kind: family
family: protocols
layers: [config, compositor, protocol, engine:drm, engine:input, engine:aura, sdk, manganese, engine:viz, engine:blink, external]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-204a969601d0, S-23c521755f22, S-2c05e7f10be8, S-3d1446ae145e, S-3dc4b8b29e37, S-3f17426ddbf7, S-4a2b900f6a8d, S-6792066180dd, S-6b79c52fc5cd, S-703c0d408b29, S-8c438f028a0f, S-90a8684915e7, S-9c59f8ebc0df, S-b23d6954c82b, S-d5b15decb594, S-e77ffff40807, S-ed569f765e51, S-eed856b1923f]
---

# Client and protocol contracts

69 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 65 same; 0 scroll-changed; 4 scroll-only; 0 sway-only.
- Domicile coverage: 2 landed; 31 partial; 22 absent; 14 unverified; 0 accounting aliases/exclusions.
- Mechanism layers: compositor 65; config 3; engine:aura 2; engine:blink 2; engine:drm 7; engine:input 3; engine:viz 3; external 6; manganese 6; protocol 58; sdk 6.
- Feature: `protocols/coverage-map`.

## Donor semantics

Native rows preserve client-state negotiation, configured-size synchronization, hidden/unresponsive clients, popups, capture trees, frame pacing, output damage and protocol advertisement. These are distinct from the shell's spatial layout and from each other. A declared global or method does not establish every lifecycle or interaction guarantee. [source] [Native transaction path](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L2239).

This family keeps the donor source anchors and an explicit unverified disposition when the corresponding current native-source map has not yet supplied evidence. The presence of DOM placement alone is not used to infer native protocol behavior. [source] [Public shell boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L50).

Current Domicile machinery has a narrower synchronization unit: a parent surface and its synchronized subsurfaces commit together. Named submitted frames and opt-in frame synchronization keep shell placement distinct from client readiness; a client that has not submitted a frame does not hold the page. The engine-frame and presentation callback fallback deadlines cover a different direction of synchronization. These documented mechanisms do not establish a multi-client window-manager readiness transaction. [source] [Commit unit and submitted frames](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L45) · [Callback deadlines](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L145).

Popup constraints use parent-relative geometry, with a donor-specific spatial boundary. Sway translates physical-output bounds into the toplevel parent's coordinates. Scroll uses workspace bounds divided by workspace and per-view content scale, and tags the popup with its workspace during global overview. Both apply constraints on initial commit and reposition; neither path here proves a Domicile popup contract. [source] [Sway constraint box](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/xdg_shell.c#L44) · [Scroll constraint box](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/xdg_shell.c#L45).

Scroll marks the Lua view as unmapped before invoking the Lua unmap callback and emitting the native unmap signal. It then destroys foreign handles and removes trail/focus-ring membership. Exit-animation retention is separate from those public handles. Pointer constraints tied to the view are cleared on every seat before the dirty transaction commit and final surface-reference removal. Callback read validity is a separate Lua-lookup contract. [source] [Unmap ordering](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1117) · [Constraint cleanup](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1186).

The donor's render budget has two stages: output composition timing and the per-window instruction to render before that composition. The per-window setting has an effect only while a per-output budget is enabled on the window's current output; its off mode tells the client to render immediately after refresh. These controls are distinct from waiting for configured client geometry and from Domicile's engine-frame/presentation fallback deadlines. [source] [Per-client budget](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L646) · [Domicile callback deadlines](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L145).

The donor pacing implementation places output rendering at predicted refresh minus its output budget, and surface callbacks additionally subtract the view budget. Missing budgets or insufficient remaining time select immediate work. These controls differ from engine/presentation fallback deadlines and from multi-client configure readiness. [source] [Pacing budgets](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L177) · [Pacing budgets](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L320) · [Pacing budgets](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L193) · [Pacing budgets](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L344).

Later client natural width/height changes do not produce a Manganese floating-resize action. Changing minimum/maximum limits follow a different path: the reducer clamps a single-native-window floating box after accepted actions, while multiwindow floating groups skip the clamp. Initial natural-size placement is separate from both. [source] [Toplevel diff](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-changes.ts#L46) · [Reducer boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1175) · [Limit application](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2574) · [Single-window limit](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/floating/float.ts#L201).

Current Manganese clipping is conditional: workspace-switch motion sets a rectangular CSS clip to that screen; resting windows have no such clip, allowing a dragged float to span screens. The window wrapper applies this policy around its bar and content. That shell animation contract does not establish Scroll’s continuously overflowing tiled-layout clipping or the complete native render/input invariant. [source] [Frame wrapper](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/WindowFrame.tsx#L79) · [Conditional screen clip](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-styles.ts#L218).

Native Domicile presentation feedback selects the first entered output in output-list order; density separately selects the display with the largest overlapping area. Those selection rules do not establish Scroll’s fastest-nonsuspended-output frame-callback pacing. Capture-ready timestamps are sampled after optional fence completion and staging-buffer copying, with no engine presentation timestamp in that payload. Delivery readiness, physical presentation and callback pacing remain distinct. [source] [Presentation feedback](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/frame_pacing.rs#L134) · [Density selection](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screens.rs#L752) · [Capture delivery](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/mod.rs#L308) · [Timestamp](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/mod.rs#L483).

The current native-list diff emits missing identifiers as AppClosed after appearances and changes. The shell removes popup records separately. Window removal clears a matching drag identifier and removes that window from background, scratchpad, window and workspace membership. That shell state cleanup does not establish foreign-handle destruction or every native pointer/constraint cleanup on protocol unmap. [source] [Disappearance diff](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-changes.ts#L18) · [Popup/window distinction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1290) · [Window cleanup](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1933).

An empty submit-damage rectangle denotes the whole box in Domicile’s documented frame contract. Partial mapping requires the prior box, crop and buffer size to match without buffer rotation or scaling; other commits use full-box damage. The document leaves physical stale-pixel correctness unverified. These damage-region rules do not establish when a whole output stops requesting frames for an undamaged scene. [docs] [Damage contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L351).

Native-record updates affect only the matching application identifier. The shallow field comparison preserves unchanged record references and returns the original state from that update path when no record changed. This updater is separate from reducer-wide floating-size limiting. [source] [Record update scope](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2085) · [Field equality](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2131).

## At the pin

The pinned native map advertises primary selection version 1. That positive registration evidence does not establish the donor startup enable/disable configuration control; the configurable policy remains unverified. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L818) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/lib.rs#L227).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-2c05e7f10be8 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-3d1446ae145e · 2026-11-05 · Scroll reference at the donor revision.
- S-3dc4b8b29e37 · 2026-11-05 · Scroll reference at the donor revision.
- S-3f17426ddbf7 · 2026-09-21 · sway reference at the donor revision.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-6792066180dd · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-6b79c52fc5cd · 2026-11-05 · Scroll reference at the donor revision.
- S-703c0d408b29 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-90a8684915e7 · 2026-09-21 · sway reference at the donor revision.
- S-9c59f8ebc0df · 2026-11-05 · Scroll reference at the donor revision.
- S-b23d6954c82b · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-e77ffff40807 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-ed569f765e51 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-eed856b1923f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
