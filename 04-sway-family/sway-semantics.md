---
id: sway-family/sway-semantics
title: Sway contracts beside the scroll layout
kind: collection
layers: [config, manganese, any-shell]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [bucket:domicile, evidence:source, scroll-parity:baseline]
sources: [S-9fe5a8a3f191, S-633706e78879, S-7b4503a0d3e1, S-32b5544fb772, S-9ed114f97663, S-23c521755f22, S-d5b15decb594, S-515bf507459b, S-90a8684915e7, S-245500bcf4dd, S-8c438f028a0f, S-1b032775d560, S-017f66cda9c3, S-16174e406a0b, S-d1e267110753, S-759448a412d0]
---

# Sway contracts beside the scroll layout

Sway's command set covers window identity, matching, modes, workspace placement and external clients as well as layout. Scroll preserves much of that surface while changing the layout model. The [scroll map](scroll-parity/README.md) records each row; this page groups the contracts.

## Window and workspace contracts

| Contract | Sway behavior | Domicile at the pin | Feature |
|---|---|---|---|
| Marks | Named marks label containers, including groups; each mark string is unique. Add, replace and toggle change a container’s marks. [source] [mark targets](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/mark.c#L20) · [literal identifiers](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/container.c#L1635) | The documented Manganese command list has no general mark commands. [source] [command inventory](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L38) | [Marks and advanced marking](../06-features/windows/marks-and-advanced-marking.md) |
| Scratchpad | Showing repeatedly traverses the scratchpad's windows. [source] [scratchpad](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L338) | Manganese cycles scratchpad windows in a modal box whose default follows the screen margins, subject to a single native client’s min/max limits. [source] [scratchpad behavior](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L187) · [native size limits](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2574) | [Floating and scratchpad](../06-features/layout/floating-and-scratchpad.md) |
| Matching rules | Criteria combine attributes, including app identity, title, mark, workspace, focus and urgency; rules run as matching windows appear. [source] [criteria](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L977) | General criteria commands are not documented; initial floating placement can match app and title expressions. [docs] [placement rules](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L204) | [Criteria and window rules](../06-features/windows/criteria-and-window-rules.md) |
| Workspace homes | Named workspaces have ordered output candidates and previous/next/back-and-forth selection. [source] [workspace commands](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L927) | Ten workspaces span the desktop; each appears on at most one screen. A populated hidden workspace keeps its home while that screen remains, otherwise it is rehomed to the focused screen. [source] [workspace model](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L45) · [home update](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2546) | [Workspace model and homes](../06-features/workspaces/workspace-model-and-homes.md) |
| Binding modes | A named mode carries bindings; binding flags distinguish release, repeat, lock state, input device and shortcut inhibition. [source] [binding semantics](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L435) | Manganese keeps one desk-wide mode; an optional exported bar item displays it. [source] [shared mode](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L189) · [docs] [mode item](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L85) | [Binding modes](../06-features/bindings-and-modes/binding-modes.md) |

## Mark targets and matching scope

The mark command accepts a container without requiring a client view. Direct lookup compares literal mark text and searches output containers, hidden scratchpad trees and saved fallback workspaces. Command criteria instead apply a regular expression for `con_mark`. View-backed targets run the view matcher; a non-view container enters the container-predicate branch when `con_mark` or `con_id` is present. Per-view rule evaluation uses its own matching path. [source] [Command target](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/mark.c#L20) · [Literal lookup](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/container.c#L1667) · [Traversal domains](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/root.c#L246) · [Criteria collector](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/criteria.c#L500) · [View rule lookup](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/criteria.c#L483)

## Rule and mode lifetimes

Sway rechecks matching rules when an xdg-shell window changes its title or application ID. It remembers each executed rule before running its command, skips that same rule on later matches, and clears the history when the view unmaps. This permits a rule to match after initial mapping without continuously reapplying its action. A command error leaves the recorded rule in place, so another matching property change does not retry that loaded rule. [source] [Property changes](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/xdg_shell.c#L351) · [Rule history](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/view.c#L558) · [Unmap boundary](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/view.c#L971)

The runtime mode command selects the current configuration mode and emits a mode event. Parsing a mode's configuration block temporarily selects that mode, then restores the previous one. [source] [Mode command](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/mode.c#L71)

## Configuration contracts beyond layout

- **Seats as configuration:** sway groups devices into independent named seats; attachment, fallback attachment, cursor settings and shortcut inhibition have separate commands. This is configuration semantics, distinct from session device access. [source] [seat definitions](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-input.5.scd#L223)
- **Output rules:** selection can use an output name, a wildcard or the focused output. Position, mode, scaling and other properties are independent. [source] [output selection](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-output.5.scd#L13) · [Domicile displays](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/DISPLAYS.md)
- **Idle and lock:** sway's per-seat idle inhibition and binding behavior while locked are separate controls. Domicile documents its idle and lock boundaries directly. [source] [seat idle sources](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-input.5.scd#L281) · [locked bindings](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L453) · [Domicile idle](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/IDLE.md) · [Domicile lock](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/LOCK.md)
- **Bar and IPC:** sway's bar reads a JSON status stream and optionally returns click events. Desktop IPC separately exposes the tree, workspaces, marks, binding state and subscriptions. [source] [bar protocol](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/swaybar/swaybar-protocol.7.scd#L7) · [desktop IPC](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-ipc.7.scd#L34) · [Manganese bar](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/shell-manganese/docs/TOP-BAR.md#L8)

The command counts and the mechanism counts differ: a single matching or mode mechanism supports several commands, while some commands cross configuration, compositor and shell state. The map records those layers per row. [inferred]

## Sources

- S-9fe5a8a3f191 · 2026-09-21 · [Sway command manual at the donor revision](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd).
- S-633706e78879 · 2026-09-21 · [Sway input and seat configuration manual at the donor revision](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-input.5.scd).
- S-7b4503a0d3e1 · 2026-09-21 · [Sway output configuration manual at the donor revision](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-output.5.scd).
- S-32b5544fb772 · 2026-09-21 · [Sway IPC requests and subscriptions at the donor revision](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-ipc.7.scd).
- S-9ed114f97663 · 2026-09-21 · [Sway bar status and click protocol at the donor revision](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/swaybar/swaybar-protocol.7.scd).
- S-23c521755f22 · 2026-11-09 · [Manganese window-management behavior at the checked pin](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md).
- S-d5b15decb594 · 2026-11-09 · [Manganese commands and default bindings at the checked pin](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md).

- S-515bf507459b · 2026-09-21 · [Sway matching-rule execution history at the donor revision](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/view.c).
- S-90a8684915e7 · 2026-09-21 · [Sway native-title and application-identity changes at the donor revision](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/xdg_shell.c).
- S-245500bcf4dd · 2026-09-21 · [Sway mode selection and configuration lifetime at the donor revision](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/mode.c).
- S-8c438f028a0f · 2026-11-09 · [Manganese desk-wide reducer state at the checked pin](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts).
- S-1b032775d560 · 2026-11-09 · [Binding adaptation and modal command filtering at the checked pin](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/useKeybindings.ts).

- S-017f66cda9c3 · 2026-09-21 · [Sway mark target and add/replace/toggle handling at the donor revision](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/commands/mark.c#L20).

- S-16174e406a0b · 2026-09-21 · [Sway literal mark storage and lookup at the donor revision](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/container.c#L1635).

- S-d1e267110753 · 2026-09-21 · [Sway container traversal domains at the donor revision](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/root.c#L246).

- S-759448a412d0 · 2026-09-21 · [Sway view and container criteria matching at the donor revision](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/criteria.c#L168).
