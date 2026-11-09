---
id: sway-family/scroll-parity/lua-scripting
title: Lua scripting
kind: family
family: ipc-scripting
layers: [any-shell, sdk]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-0d9c2ee30def, S-12e11cee7f48, S-204a969601d0, S-23c521755f22, S-333f76cea05b, S-36bae2cd0f28, S-4a2b900f6a8d, S-541270a86f10, S-6b79c52fc5cd, S-a3aab1e75b83, S-b5101babba40, S-c0a5420d3a23, S-d5b15decb594, S-e77ffff40807]
---

# Lua scripting

127 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 0 same; 0 scroll-changed; 127 scroll-only; 0 sway-only.
- Domicile coverage: 8 landed; 41 partial; 77 absent; 0 unverified; 1 accounting aliases/exclusions.
- Mechanism layers: any-shell 127; sdk 127.
- Feature: [ipc-scripting/lua-and-shell-scriptability](../../06-features/ipc-scripting/lua-and-shell-scriptability.md).

## Donor semantics

Scroll embeds a Lua module for observations, commands and event callbacks. The API covers view identity and metadata, container geometry and parent/child relations, workspaces, outputs, scratchpad membership, animation inspection, process launch, JSON conversion and logging. [source] [API](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L2110).

Each script key has a separate persistent-state table, shared by repeated calls even when their arguments differ. All scripts run inside the configuration's single interpreter with its standard libraries. A view identifier lasts for the mapped lifetime; container identifiers can change during restructuring. Access through an obsolete workspace/container identifier returns a safe default. [source] [State and identity](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L2147).

Command execution accepts an explicit window/workspace context and returns operation results. Immediate transaction commit is the default and ends an ongoing animation; the option can be disabled. Process launch returns its process identity and activation token. Callbacks associate event names with functions and user data. [source] [Commands and launch](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L2225) · [Callbacks](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L2536).

The donor demonstrates workspace-creation callbacks that run commands and configure region splits. Its Lua evaluation request and REPL expose the same scripting surface through IPC. These are published donor examples. [source] [Workspace callback example](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1880) · [Evaluation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1946).

Domicile publishes a different composition mechanism: a JavaScript shell module, typed state/events, configurable Manganese options and reusable packages. The reusable-module route is stated in the public shell guides. [source] [Shell module](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L34) · [Manganese packages](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md#L71).

## Lifetime and callback boundaries

A configuration creates one Lua interpreter with the standard library and closes it when that configuration is freed. Persistent script state is keyed by script path or evaluation name, with call arguments supplied separately. [source] [Interpreter lifetime](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config.c#L177) · [Script keys](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/lua.c#L115).

The mapped-window callback runs after criteria but before focus selection; initial fullscreen processing and arrangement have already happened. Callback execution is protected, with errors logged and cleared from the Lua stack. Eleven callback types cover window/workspace lifecycle, IPC, jump completion and Lua-command completion. The command-completion callback observes Lua-executed commands, rather than every compositor command. [source] [Mapping order](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1063) · [Protected callbacks](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L125) · [Event registry](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L1867).

Command results are integer statuses or error strings. Suppressing immediate commit leaves the dirty transaction for later commit; it is not an acknowledgment of display presentation. IPC evaluation first attempts expression parsing and returns result values, captured output and success/error state. [source] [Command results](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L431) · [Evaluation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L2229).

The dedicated Lua callback registry covers window/workspace lifecycle, IPC, jump completion and Lua command completion. It has no animation-frame or gesture-update callback kind. Target geometry, animated geometry and state-change snapshots are separate observations; a state-change event does not by itself establish presentation-progress delivery. [source] [Callback kinds](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L1867).

An unmapped view is not immediately a destroyed node. The unmap callback receives a container identifier after the mapped flag becomes false; Lua lookup and title/application/process metadata access use the node registry without a mapped guard. Final container destruction removes the identifier. This bounds the initial callback lookup without promising every later operation or callback lifetime. [source] [Unmap sequence](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1117) · [Registry lookup](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L372) · [Mapped query and metadata](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L617) · [Callback argument](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L2132) · [Final destruction](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/container.c#L521).

The Lua command wrapper normally commits dirty state before running its command-end callbacks; commit=false omits that immediate commit. Those callbacks apply to Lua-issued commands, rather than every command entering through IPC. A container or nil target passes no seat to the shared command executor and therefore uses the default seat. A workspace target uses the current seat and sets its raw focus to that workspace. These are explicit calling-scope rules, separately from callback registry lifetime. [source] [Lua target and commit scope](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L431) · [Default seat](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands.c#L276).

The file and named-buffer Lua evaluators save and restore the explicit container-context pointer on normal completion and load/script-error returns. That pointer scope is separate from the Lua command's immediate-commit option; it does not establish generic nested transaction-delay save and restore. [source] [File context](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/lua.c#L100) · [Named-buffer context](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/lua.c#L154).

Geometry observations are distinct: destination geometry reads pending fields; animated geometry uses scene coordinates or current-position fallback and chooses animated versus current dimensions from global animation state. A separate call returns raw variables and a per-container animation flag. Geometry getters return nil for missing/unresolved containers, while fullscreen getters return none/disabled, indistinguishable from a valid off state. [source] [Geometry observations](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L1057) · [Animation variables](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L1119) · [Fullscreen defaults](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L1146).

## Read models and modifiers

The child getter returns immediate child node identifiers. The view getter returns a leaf itself or only view-bearing immediate children at their existing indices, without recursive traversal; missing or unresolved handles return empty tables. Workspace mode readback likewise returns an empty table for an unresolved handle. [source] [Children and views](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L1276) · [Mode readback](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L1413).

Mode updates apply recognized typed fields independently. Missing, wrongly typed and unknown values leave the corresponding modifier unchanged. Both readback and update expose fitting policy as nofit, fitsplit or fitfraction; the mode getter’s none value is not accepted by the setter. [source] [Typed updates](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L1496) · [Fitting field](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L1529).

The virtual-output query omits internal split nodes. Each leaf descriptor carries current, split and usable rectangles, separate visible and active flags, parallel workspace-node/name lists and an active-workspace identifier. Its maximized flag is copied from the whole physical output into every descriptor; it does not by itself identify the visible maximized leaf. [source] [Region descriptors](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L1732).

The parent-view helper follows process ancestors until it finds a managed view. This is independent of Wayland transient-parent identity. Environment lookup likewise uses the view’s process identity and returns nil when the requested metadata is unavailable. [source] [Environment lookup](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L743) · [Process ancestry](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L782).

Invalid readbacks differ by method. Width/height and fractional dimensions return zero; floating, sticky and scratchpad flags return false; rectangle and opacity getters return nil. Urgency returns false without an argument but nil when a supplied view cannot be resolved. Opacity selects its animated value while that variable is animating, otherwise its pending value. [source] [Urgency defaults](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L810) · [Flags and opacity](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L933) · [Numeric defaults](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L997).

JSON conversion does not preserve a distinct null sentinel in Lua tables. Decoding null pushes nil, which removes the corresponding table key or array index. Encoding an empty table produces an array; array detection expects numeric keys in traversed order 1 through n. Object mode retains string/numeric keys and skips other key types. [source] [Encoding](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L177) · [Object keys](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L238) · [Decoding](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L280).

The pending-transaction query includes queued transactions, pending transactions and dirty nodes awaiting transaction creation. It is separate from animation activity and does not acknowledge physical presentation. [source] [Pending query](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L2018).

## Lineage

- 2025-07-16: published examples explore urgency-driven focus, related-window placement, moving selections, alternate arrangements, parent swallowing, workspace initialization and restoring a prior maximize box. [docs] [Public examples](https://github.com/dawsers/scroll/issues/35#issuecomment-3046622595).
- 2026-01-07: an example sends structured script data to external subscribers, including bars, through the Lua event. [docs] [Public example](https://github.com/dawsers/scroll/discussions/48#discussioncomment-15432621).
- 2026-04-09: a jump-completion example selects a second window and exchanges it with a window remembered before jump began. [docs] [Public example](https://github.com/dawsers/scroll/discussions/48#discussioncomment-16508595).

- 2026-05-18: a published callback reacts to later floating-state changes, which creation-time matching alone does not track. [docs] [Public example](https://github.com/dawsers/scroll/discussions/278#discussioncomment-16958740).

## At the pin

Not addressed at the pin: the published shell contract uses a JavaScript module and typed namespaces; the complete Manganese command table declares no embedded Lua runtime or callable Lua members. [source] [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L50) · [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35) · [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md#L71).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-12e11cee7f48 · 2026-11-05 · Scroll reference at the donor revision.
- S-333f76cea05b · 2026-11-05 · Scroll reference at the donor revision.
- S-36bae2cd0f28 · 2026-11-05 · Scroll reference at the donor revision.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-541270a86f10 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-6b79c52fc5cd · 2026-11-05 · Scroll reference at the donor revision.
- S-a3aab1e75b83 · 2026-05-18 · public floating-state callback example.
- S-b5101babba40 · 2026-11-05 · Scroll reference at the donor revision.
- S-c0a5420d3a23 · 2026-11-05 · Scroll reference at the donor revision.
- S-e77ffff40807 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
