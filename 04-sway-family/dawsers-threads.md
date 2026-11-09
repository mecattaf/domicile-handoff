---
id: sway-family/dawsers-threads
title: Scroll discussion findings
kind: collection
layers: [external, any-shell]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [scroll, sway]
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, donor:scroll, evidence:docs, scroll-parity:baseline]
sources: [S-b948b453611f, S-03db46ad9d28, S-4a2b900f6a8d, S-2d260845be4d, S-b5101babba40, S-c0a5420d3a23, S-333f76cea05b, S-862b24b56e86, S-cc0774ece77f, S-5f5a6c3110a9, S-3025ab8ea45b, S-9aef30917472, S-7f2d34d0bfe7, S-9c59f8ebc0df, S-6b79c52fc5cd, S-60eaeae815c0, S-36bae2cd0f28]
---

# Scroll discussion findings

The preserved discussion log records the boundaries between scroll's compositor mechanisms, Lua policy and external shell behavior. Its historical state is dated 2026-10-23; the [scroll map](scroll-parity/README.md) checks behavior against the public scroll and Domicile pins. [docs] S-b948b453611f

## Semantics retained from the discussions

| Topic | Contract clarified by the discussion | Evidence at the scroll pin |
|---|---|---|
| App-relative placement | Window identity and layout policy can be supplied through the Lua API rather than a growing set of relative criteria. | [placement discussion](https://github.com/dawsers/scroll/issues/32) · [Lua command and API](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1938) [docs] |
| Script callbacks | Scripts react to compositor state and can set per-window behavior. This differs from importing a shell toolkit. | [Lua announcement](https://github.com/dawsers/scroll/issues/35) · [Lua surface](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1954) [docs] |
| Invocation context | Evaluating a Lua buffer over IPC has a result contract and execution context, distinct from loading a script file. | [evaluation discussion](https://github.com/dawsers/scroll/issues/347) · [evaluation request](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-ipc.7.scd#L1839) [source] |
| Saved arrangements | Persisted arrangement templates and access to an existing saved arrangement are separate capabilities. | [template discussion](https://github.com/dawsers/scroll/discussions/384) · [arrangement requests](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-ipc.7.scd#L1596) [docs] |
| Process and window identity | Process identity and window identity differ: an existing application process can create a window requested by another process. | [activation discussion](https://github.com/dawsers/scroll/issues/339) · [process API](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L2240) [docs] |
| Window geometry | Content geometry and visibility are useful separately from the outer window rectangle. | [content rectangle discussion](https://github.com/dawsers/scroll/issues/340) · [tree reply](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-ipc.7.scd#L312) [docs] |
| Minimize | Client minimization has global and per-window controls. Taskbar restore re-inserts the window into tiling rather than restoring its previous floating state. | [minimize discussion](https://github.com/dawsers/scroll/issues/228) · [per-window minimize](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L881) [source] |
| Animation opt-out | A binding can suppress its own animation. A later jump selection or command from a spawned script belongs to another transaction. | [binding discussion](https://github.com/dawsers/scroll/issues/360) · [binding flags](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1153) [docs] |
| Change events | Scroller changes, trail changes and Lua-emitted data have separate event payloads. | [scroller status discussion](https://github.com/dawsers/scroll/issues/308) · [added events](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-ipc.7.scd#L2338) [source] |
| Resize completion | A request for a resize-end notification is not equivalent to observing surface commits. The historical log kept that distinction open. | [resize discussion](https://github.com/dawsers/scroll/issues/349) · S-b948b453611f [docs] |

## Script lifetime at the pin

Script invocations pass arguments independently of the state keyed by a script's path or evaluation name. The script-local tables share the loaded configuration's Lua interpreter and standard libraries; replacing the configuration closes that interpreter. [source] [script state](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/lua.c#L7) · [configuration lifetime](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config.c#L177)

The command result returned inside Lua is an array of status integers or error strings. IPC evaluation instead reports success, returned values and captured output. The Lua `command_end` callback covers Lua-executed commands only. [source] [command contract](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L431) · [IPC evaluation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L2229)

Unmap callbacks receive the container's node identifier after the view's mapped flag becomes false. Lua conversion resolves that identifier through the node registry; metadata accessors do not use the mapped flag as their guard. Final container destruction removes the registry entry. Mapped state and identifier lookup are therefore different checks; this does not establish a general lifetime guarantee for later operations. [source] [Callback order](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1117) · [Callback argument](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L2132) · [Lookup and mapped test](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L372) · [Metadata accessors](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L617) · [Final removal](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/container.c#L521)

File and named-buffer Lua commands save the previous explicit container-context pointer and restore it on their normal, load-error and script-error return paths. This is separate from the immediate-commit option of a command invoked inside Lua. [source] [File command context](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/lua.c#L100) · [Named-buffer context](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/lua.c#L154)

## Published Lua policy examples

Public examples use callbacks for urgency-driven focus, app-relative placement, bulk relocation, alternative arrangements, hiding a parent while its child is present, workspace initialization and restoring maximized geometry. [docs] [example collection](https://github.com/dawsers/scroll/issues/35#issuecomment-3046622595) · [arrangements](https://github.com/dawsers/scroll/issues/35#issuecomment-3049737169) · [geometry restoration](https://github.com/dawsers/scroll/issues/35#issuecomment-3080616692)

Other examples treat jump labels as a window picker followed by a swap, send structured script state to external bars, and fill bounded columns through map callbacks. These are uses of the API, separate from the command inventory. [docs] [jump picker](https://github.com/dawsers/scroll/discussions/48#discussioncomment-16508595) · [script events](https://github.com/dawsers/scroll/discussions/48#discussioncomment-15432621) · [column policy](https://github.com/dawsers/scroll/discussions/277#discussioncomment-16984201)

## Binding animation scope

At the pin, the binding handler chooses its no-animation commit after executing the command and emitting a successful binding event. The flag marks the dirty transaction being committed. Transaction processing temporarily disables animation setup and then restores the configured enable state. This is a transaction-scoped control, separate from changing the global animation setting. [source] [Binding commit order](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/bind.c#L638) · [Dirty-work boundary](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L2514) · [Configuration restoration](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L2108)

## Renderer and integration boundaries

The effects discussions separate rounded clipping, shadow and dim behavior from background sampling and multi-pass blur. The [swayfx comparison](swayfx.md) records the source constraints; the [sway-next record](sway-next.md) distinguishes present code from unobserved GPU output. [docs] S-03db46ad9d28 · [blur discussion](https://github.com/dawsers/scroll/issues/3)

Socket compatibility, desktop identity and session startup appear in the log as integration contracts. They are separate from the scrolling layout. [docs] S-b948b453611f · [socket discussion](https://github.com/dawsers/scroll/issues/36) · [session discussion](https://github.com/dawsers/scroll/issues/43)

## Where it touches Domicile

Domicile documents a different scripting route: [reuse Manganese](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md#L71) or [write a shell](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md). These are public module and shell contracts. Scroll's Lua callbacks and IPC payloads are the donor contracts in this comparison. [docs]

The corresponding feature pages cover [scriptability](../06-features/ipc-scripting/lua-and-shell-scriptability.md), [criteria](../06-features/windows/criteria-and-window-rules.md), [marks](../06-features/windows/marks-and-advanced-marking.md) and [feel](../06-features/feel-and-animation/scroll-feel-parity.md).

## Lineage

- 2025..2026: public discussions clarified script policy, matching, rendering and session boundaries. [docs] S-b948b453611f
- 2026-10-23: the preserved catalog linked those discussions to the sway-next technical state. Historical dispositions are not current Domicile statuses. [docs] S-b948b453611f

## Sources

- S-4a2b900f6a8d · 2026-11-05 · [Scroll command and Lua manual at the donor revision](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd).
- S-2d260845be4d · 2026-11-05 · [Scroll IPC requests and event payloads at the donor revision](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-ipc.7.scd).
- S-b948b453611f · 2026-10-23 · Preserved scroll discussion and capability catalog
- S-03db46ad9d28 · 2026-10-23 · Compatibility and renderer boundaries
- S-862b24b56e86 · 2025-07-16 · Public callback and layout examples
- S-cc0774ece77f · 2026-10-22 · Preserved public scripting discussion and event examples
- S-5f5a6c3110a9 · 2026-07-20 · Public grid and navigation script discussion
- S-3025ab8ea45b · 2026-10-22 · Public process and window matching discussion
- S-9aef30917472 · 2026-07-21 · Public animation suppression scope discussion

- S-7f2d34d0bfe7 · 2026-11-05 · [Scroll binding dispatch and transaction flags at the donor revision](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/bind.c#L638).
- S-9c59f8ebc0df · 2026-11-05 · [Scroll transaction and animation state at the donor revision](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L2514).

- S-333f76cea05b · 2026-11-05 · [Scroll Lua observations and callback dispatch at the donor revision](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L2132).
- S-60eaeae815c0 · 2026-11-05 · [Scroll node-identifier registry at the donor revision](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/node.c#L56).
- S-36bae2cd0f28 · 2026-11-05 · [Scroll container destruction and retained state at the donor revision](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/container.c#L521).

- S-b5101babba40 · 2026-11-05 · [Scroll script state and explicit container context at the donor revision](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/lua.c).
- S-c0a5420d3a23 · 2026-11-05 · [Scroll configuration and interpreter lifetime at the donor revision](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config.c).
- S-6b79c52fc5cd · 2026-11-05 · [Scroll view mapping and unmap order at the donor revision](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c).
