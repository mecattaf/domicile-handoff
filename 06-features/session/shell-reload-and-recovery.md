---
id: session/shell-reload-and-recovery
title: Shell reload and component recovery
kind: feature
family: session
layers: [engine:browser, host, compositor, config, any-shell, session]
domicile_status: landed
domicile_refs: [cprussin/domicile#1888, cprussin/domicile#1896]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll]
rows: []
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:session, status:landed, evidence:source, scroll-parity:baseline]
sources: [S-09fd0ca98c14, S-1fb7627f016e, S-20e287046b4a, S-2465409327d2, S-2ee24d100a88, S-396bf2ab3542, S-39c541407074, S-3c7ed93e0c4b, S-411dca61faa5, S-4ed3510bbb79, S-5523ef128ac2, S-612a99e2b687, S-6167ac277bd3, S-625c4e474be6, S-660caed2860b, S-695f43c6be84, S-7f27df8cd26d, S-85c672237a99, S-866b9f3f829d, S-873afc9d9e8b, S-8ae528bbecc2, S-9bce4d22631c, S-a30cab548bc0, S-a3b9530bf2b3, S-b5968ceb9b74, S-c6f3a38054b8, S-c9366e4f4442, S-e34b121eace4, S-ecfec2f17332, S-f6401f8c13cc]
---
# Shell reload and component recovery

The launcher watches shell configuration and supervises the engine and compositor separately. Reloading the page, restarting the browser engine and restarting the Wayland server have different persistence boundaries. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/restart.rs#L1) [source]

## Behavior

- Configuration watch follows edits below the module directory, excluding dot directories and dependency folders, and waits for an edit burst to settle. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/config_watch.rs#L17) [source]
- The binary contract retains native Wayland connections and their last frame after engine exit. Compositor exit drops those connections; its documented relaunch covers applications launched by the shell. This describes connection and retained-buffer lifetime. [Component contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/THE-DOMICILE-BINARY.md#L226) [docs]
- The supervisor counts engine, compositor and whole-desktop restarts independently, with bounded backoff. An engine that reports unusable rendering shortly after startup ends the run instead of repeating the same startup. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/restart.rs#L12) [source]

- A replacement that reports an error restores the previous shell source when its root or module differs. A rebuild at the same path has no saved copy of the previous file bytes and leaves the error screen. [#1888](https://github.com/cprussin/domicile/pull/1888) landed that replacement rollback. [source] [Replacement decision](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/shell_swap.cc#L66) · [Saved source boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/shell_swap.h#L56)
- [#1896](https://github.com/cprussin/domicile/pull/1896) made an unusable rendering startup fatal rather than repeatedly restarting it. [docs]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Configuration reload updates the running compositor | A full compositor process replacement has a different client boundary | [pinned reload command](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/reload.c#L54) |
| scroll | Retains sway runtime reload | Shell policy state is separate from process and client connection state | [pinned reload command](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/reload.c#L54) |

## What the notes found

- The useful distinction is connection lifetime versus shell layout state. Surviving native processes do not by themselves demonstrate retained workspaces, tabs or window geometry. [inferred]
- The console-return trial records an initial displayed frame followed by display-configuration errors on console return. It does not establish a Domicile recovery result. S-411dca61faa5 · 2026-10-23 · Console return and display-state trial [docs]
- 2026-11-09: The closed-surface resource helper pairs retained imports with a generation token. A stale timer cannot remove a newer linger reusing the same surface identifier, and engine loss clears the retained set. This is source lifetime handling, not measured cleanup timing [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/engine_lingering.rs#L38> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/engine_lingering.rs#L61>
- 2026-11-09: The launcher propagates an initial configuration-module evaluation error before desktop setup. Live reevaluation stages and renames a replacement configuration only after evaluation succeeds; raw JSON configuration follows a separate path [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/bin/domicile.rs#L614> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/bin/domicile.rs#L1310>
- 2026-11-09: The launcher installs SIGINT/SIGTERM handlers that set its stop marker. At the desktop entry point, Ending::Stopped selects ExitCode::FAILURE; this states control flow rather than a new shutdown measurement [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/supervise.rs#L310> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/bin/domicile.rs#L820>
- 2026-11-04: Domicile docs state there is no reload or exit command; Settings pages write config as changes are made and the desktop reloads it [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L168> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SETTINGS.md#L8>
- 2026-11-09: Recoverable SDK subscriptions re-register under a new call identifier after compositor loss. Once restarted, streams with a missed-events marker report that intervening changes were not heard [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/system-calls.ts#L635>
- 2026-11-09: The startup.commands setting lists argv programs that run once at desktop startup and not on shell reload [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/RUNNING-A-DESKTOP.md#L54>
- 2026-11-06: A shell reload kept every window open but returned them all to workspace 1; see [window placement across shell reloads](../layout/layout-retention-across-reload.md) and living row L-01. [measured]
- 2026-11-04: After the engine process was killed on a headless desk, the desk returned within about two seconds with browser, terminal and file-manager windows intact and keys working; a side-by-side layout came back as tabs [measured]
- 2026-11-05: Replacing the watched config link reloaded the shell automatically while the engine, compositor, native terminals and browser guest kept their processes; replacing the loaded engine needed a session restart [measured]
- 2026-11-07: The shell page survives a compositor restart without remounting; state that records compositor identity is refreshed explicitly by the shell. [measured]
- 2026-11-04: A shell reload followed by an engine crash left the replacement engine without a logged compositor rejoin, and the rescue chord launched nothing; without the preceding reload the desk recovered [measured]

## Where it touches Domicile

- `session` owns source watching and component restart policy. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/config_watch.rs#L21) [source]
- `compositor` continues serving native clients while the engine is replaced; the tty can be dark during that interval. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/restart.rs#L3) [source]
- `any-shell` owns retained placement and page state; the feature map treats those semantics separately. [inferred]
- Status at the pin: landed. The [startup-command contract](startup-commands.md) separately describes program launch and shell reload. [Desktop contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/RUNNING-A-DESKTOP.md#L54) [docs]

## Lineage

- 2026-10-23: the donor console trial separates a live process from a recovered visible display. S-411dca61faa5 · 2026-10-23 · Console return and display-state trial [docs]
- 2026-11-09: the pinned launcher supervises engine and compositor under independent restart counters. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/restart.rs#L16) [source]

## Sources

- S-f6401f8c13cc · 2026-11-09 · Desktop startup, reload and recovery contract.

- S-8ae528bbecc2 · 2026-11-07 · Unusable rendering startup merge record.

- S-a30cab548bc0 · 2026-11-06 · Replacement-shell rollback merge record.

- S-1fb7627f016e · 2026-11-09 · Replacement source lifetime.

- S-b5968ceb9b74 · 2026-11-09 · Replacement-shell rollback decision.

- S-7f27df8cd26d · 2026-11-09 · Pinned donor implementation locator.

- S-5523ef128ac2 · 2026-11-09 · Pinned donor implementation locator.

- S-2465409327d2 · 2026-11-09 · Configuration directory watch and edit coalescing.
- S-2ee24d100a88 · 2026-11-09 · Independent engine and compositor restart supervision.
- S-411dca61faa5 · 2026-10-23 · Console return and display-state trial.
- S-695f43c6be84 · 2026-11-07 · Component lifetime and native client continuity study.
- S-9bce4d22631c · 2026-11-09 · Desktop startup and configuration reload contract.
