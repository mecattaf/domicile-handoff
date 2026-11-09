---
id: session/startup-commands
title: Startup commands and shell reload
kind: feature
family: session
layers: [config, session]
domicile_status: landed
domicile_refs: [cprussin/domicile#735, docs/RUNNING-A-DESKTOP.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, niri]
rows: []
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:session, layer:session, status:landed, donor:sway, donor:scroll, donor:niri, evidence:source, scroll-parity:baseline]
sources: [S-0efe274e4561, S-25350b9112e8, S-27ea9d33b5bb, S-3e77e5b8cb37, S-3f524db52413, S-4f199c4874f5, S-4fa0f3c4914a, S-9bce4d22631c, S-aa3f2bac47f9, S-b5cbd3a24134, S-c9366e4f4442, S-e3b674100515, S-e8be1f87d087, S-e9a2068bb144, S-f6401f8c13cc]
---
# Startup commands and shell reload

Each compositor start runs its configured command vectors after publishing the session. Reloading the shell page neither repeats nor stops them. [Desktop startup contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/RUNNING-A-DESKTOP.md#L54) [docs]

## Behavior

- Empty command vectors are rejected. Execution uses no implicit shell; shell syntax is supplied through an explicit shell invocation. [Schema and validation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/startup.rs#L18) [source]
- The schema documents one launch at compositor startup and says that reload neither reruns nor stops those programs. [Startup contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/startup.rs#L12) [source]
- Startup launches are excluded from recovery records. A separate loop replays lost recorded applications; [component recovery](shell-reload-and-recovery.md) describes that boundary. [Startup launch pass](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1652) · [Recovery pass](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1664) [source]

- Spawn errors are logged without being returned from the helper. Successfully spawned children receive a waiting thread that reaps them. [Spawn result handling](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/spawning.rs#L27) [source]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | `exec` returns without launching during reload; `exec_always` uses the shared execution path | Inactive configuration and validation defer execution. The shared path joins arguments and launches `sh -c` | [Reload gate](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/exec.c#L7) · [Execution path](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/exec_always.c#L17) · [Shell launch](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/exec_always.c#L83) |
| scroll | Retains the `exec` reload gate and `exec_always` path | Startup/reload selection precedes shell-command execution; this is separate from Domicile's documented argument vectors | [Reload gate](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/exec.c#L7) · [Execution path](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/exec_always.c#L17) · [Shell launch](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/exec_always.c#L83) |
| niri @ source pin | Startup argv and explicit shell-command lists use separate helpers | The argv path uses Command directly; the shell helper supplies sh -c. Only the startup-loop paths are described here | [Startup lists](https://github.com/YaLTeR/niri/blob/9955d921ce6491e34fb74ec94f30a7cca76826e4/src/main.rs#L168) · [Startup loops](https://github.com/YaLTeR/niri/blob/9955d921ce6491e34fb74ec94f30a7cca76826e4/src/main.rs#L259) · [Argv helper](https://github.com/YaLTeR/niri/blob/9955d921ce6491e34fb74ec94f30a7cca76826e4/src/utils/spawning.rs#L65) · [Shell helper](https://github.com/YaLTeR/niri/blob/9955d921ce6491e34fb74ec94f30a7cca76826e4/src/utils/spawning.rs#L92) |

## What the notes found

- Reload policy and command representation are independent. The donor command path invokes a shell; the Domicile desktop contract specifies argument vectors. The command representations have different quoting and expansion semantics. [Donor shell path](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/exec_always.c#L40) [source] · [Domicile argument vectors](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/RUNNING-A-DESKTOP.md#L54) [docs]
- Per-application process scopes are documented separately in the [session facts](facts.tsv). Process scopes describe membership separately from startup and component recovery. [Application scopes](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/RUNNING-A-DESKTOP.md#L64) [docs]
- 2026-11-04: A config file placed directly in a Nix store directory made the shell loader watch that whole parent recursively: watches grew past 25,000 and no engine child started; a directory-backed config path bounded the watch [measured]
- 2026-11-04: Upstream startup commands in the shell configuration launched Chrome, a terminal and a file manager once per compositor start; reloading the shell did not repeat them, and Chrome single-instance reuse made later launches reuse the running browser [docs]

## Where it touches Domicile

- `config` names and validates startup argument vectors. The compositor runs the list after session publication; reload restatement has no startup-command arm. [Configuration contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-config/src/startup.rs#L12) · [Compositor startup](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1652) · [Reload restatement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/restatement.rs#L94) [source] · [Desktop guide](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/RUNNING-A-DESKTOP.md#L54) [docs]
- Status at the pin: landed for startup argument vectors and the reload contract. [#735](https://github.com/cprussin/domicile/pull/735) introduced those commands; the [pinned guide](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/RUNNING-A-DESKTOP.md#L54) and current schema document them. The source locates execution at compositor startup, separately from application recovery. [docs]

## Lineage

- 2026-09-30: [Startup argument vectors](https://github.com/cprussin/domicile/pull/735) merge upstream. [docs]

- 2026-11-09: the pinned schema and desktop guide document startup execution separately from shell reload. [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/RUNNING-A-DESKTOP.md#L54) [docs]

## Sources

- S-3e77e5b8cb37 · 2026-11-09 · Niri argv and explicit shell-command helpers.

- S-e9a2068bb144 · 2026-11-09 · Niri startup list extraction and startup loops.

- S-25350b9112e8 · 2026-11-09 · Accepted configuration field adoption.

- S-4f199c4874f5 · 2026-11-09 · Configuration reload restatement.

- S-e3b674100515 · 2026-11-09 · Spawn error handling and child reaping.

- S-3f524db52413 · 2026-11-09 · Compositor startup command and application-recovery passes.

- S-aa3f2bac47f9 · 2026-09-30 · Startup command and reload-lifecycle merge record.

- S-f6401f8c13cc · 2026-11-09 · Pinned desktop startup and reload contract.

- S-9bce4d22631c · 2026-11-09 · Documentary desktop startup and process-scope record.
- S-0efe274e4561 · 2026-11-09 · Startup argument vectors, validation and reload contract.
- S-b5cbd3a24134 · 2026-11-09 · Sway startup command reload gate.
- S-e8be1f87d087 · 2026-11-09 · Scroll startup command reload gate.
- S-27ea9d33b5bb · 2026-11-09 · Sway deferred execution and shell launch.
- S-4fa0f3c4914a · 2026-11-09 · Scroll deferred execution and shell launch.
