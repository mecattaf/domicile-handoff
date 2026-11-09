---
id: sway-family/scroll-parity/command-and-process-config
title: Command syntax and process configuration
kind: family
family: ipc-scripting
layers: [config, sdk, external, manganese, compositor, engine:viz, protocol]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-06aa410bd129, S-0d9c2ee30def, S-12e11cee7f48, S-204a969601d0, S-23c521755f22, S-278d2a39212b, S-4a2b900f6a8d, S-8c438f028a0f, S-d5b15decb594, S-e77d53738a1d, S-e77ffff40807, S-f7e6c6503485]
---

# Command syntax and process configuration

144 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 133 same; 4 scroll-changed; 7 scroll-only; 0 sway-only.
- Domicile coverage: 0 landed; 4 partial; 129 absent; 3 unverified; 8 accounting aliases/exclusions.
- Mechanism layers: compositor 5; config 122; engine:viz 1; external 139; manganese 4; protocol 1; sdk 127.
- Feature: [ipc-scripting/lua-and-shell-scriptability](../../06-features/ipc-scripting/lua-and-shell-scriptability.md).

## Donor semantics

Donor startup options, command parsing, configuration discovery, environment variables and process launch are different interfaces from window behavior. CLI diagnostics and internal coverage aliases remain accounting rows, rather than independent product features. [source] [Command conventions](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L43).

Manganese's `exec` takes an argument vector. Shell expansion can be obtained by explicitly launching a shell, but is not implicit. Its command table has no sway-compatible config reload or session-exit operation; shell loading and compositor configuration use separate Domicile interfaces. Selected wlroots backend environment variables belong to that donor dependency. [source] [Command differences](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L160) · [Shell module lifecycle](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L74).

The shared donor executor keeps a criteria-selected target set across comma-separated commands and starts a new criteria context after a semicolon. An invalid command stops the remaining list, while ordinary command failure can be recorded and followed by later commands. Native IPC commits dirty state before replying, with a single forced modeset for pending output changes. These parser/commit rules are separate from Manganese's exact argv command table. [source] [Command-list executor](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands.c#L301) · [Failure handling](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands.c#L359) · [IPC commit before reply](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/ipc-server.c#L661).

The current kill action resolves all descendants of the selected node. Native and browser clients receive close requests; registered background application windows hide instead, and overview guards can suppress keyboard dispatch. This is a closure/request contract, rather than proof that a process terminates. [source] [Selected descendants](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L328) · [Dispatch](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindows.ts#L123) · [Close requests](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindows.ts#L290).

Close scopes differ between the donors. Sway visits the current workspace when its kill command has no selected container. Scroll’s default/focused form instead does nothing without a selected container; its unfocused form requires a focused leaf, and all visits current workspaces plus scratchpad entries. Manganese supplies a narrower focused-descendant close action, with background-app hiding and an overview guard. Close requests do not establish process termination. [source] [Sway scope](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/kill.c#L15) · [Scroll scope](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/kill.c#L23) · [Manganese close](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/useWindows.ts#L290).

## At the pin

Accounting alias or excluded source surface; no independent runtime behavior counted. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-06aa410bd129 · 2026-09-21 · sway reference at the donor revision.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-12e11cee7f48 · 2026-11-05 · Scroll reference at the donor revision.
- S-278d2a39212b · 2026-11-05 · Scroll reference at the donor revision.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-e77d53738a1d · 2026-11-05 · Scroll reference at the donor revision.
- S-e77ffff40807 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-f7e6c6503485 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
