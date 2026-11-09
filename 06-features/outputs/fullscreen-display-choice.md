---
id: outputs/fullscreen-display-choice
title: Honor a native fullscreen display request
kind: feature
family: outputs
layers: [compositor, protocol, sdk, manganese]
domicile_status: landed
domicile_refs: [cprussin/domicile#1917, cprussin/domicile#1920]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: true
scroll_parity: baseline
donors: [sway, scroll]
rows: []
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:outputs, status:landed, evidence:source, scroll-parity:baseline]
sources: [S-21c064d97a59, S-3d37c6acf8e5, S-3dc4b8b29e37, S-612a99e2b687, S-80a34a7f0718, S-811bf6978f5a, S-866b9f3f829d, S-90a8684915e7, S-97f867b6b31f, S-9d1497c20da4, S-a59107f9d164, S-e77ffff40807, S-ee6cda96ee99, S-f5d75cc3208b, S-f952b7973963]
---
# Honor a native fullscreen display request

A native client that names an output in its fullscreen request is placed on that screen's current workspace. Leaving fullscreen preserves that placement. [#1920](https://github.com/cprussin/domicile/pull/1920) [docs]

## Behavior

- Only the requesting window moves. Repeated fullscreen requests preserve tiling state, and a display removed before handling is ignored. [#1920](https://github.com/cprussin/domicile/pull/1920) [docs]
- A request with no output leaves output selection to the shell. A client API naming a screen does not prove that client sends an output over Wayland. [docs]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | xdg fullscreen request carries an optional output | Compositor policy chooses placement for an absent output | [pinned xdg shell](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/xdg_shell.c) |
| scroll | Retains sway xdg fullscreen routing | Fullscreen state and scrolling layout remain separate | [pinned xdg shell](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/xdg_shell.c) |

## What the notes found

- A 2026-11-07 study traced the requested display through compositor and SDK and located the dropped field in shell handling; the fix changed neither input acquisition nor browser gesture handling. S-80a34a7f0718 · 2026-11-07 · Fullscreen output routing study [docs]
- 2026-11-09: Fullscreen requests retain an optional display identifier in the SDK; wire requests are converted without discarding that field. Delivery of the request remains separate from the shell granting state [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/window-state.ts#L32> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/system-calls.ts#L811>
- 2026-11-07: On 2026-11-07 issue #1917 and PR #1918 addressed Manganese dropping the display named by a fullscreen request. The regression test moves only the requesting window to the selected display, did not pass before the fix, and 1,432 package tests passed after. Chromium's observed set_fullscreen(nil) is a separate limitation [measured] <https://github.com/cprussin/domicile/issues/1917> <https://github.com/cprussin/domicile/pull/1918>
- 2026-11-07: On Linux, Chrome 156, asked for the second screen through the Window Management API, sent xdg_toplevel.set_fullscreen with no output, so a compositor receives nothing to honor; observed in a DRM guest running Domicile [measured]
- 2026-11-08: A fullscreen request naming a specific output was ignored in Manganese (issue #1917); PR #1918 fixed it and the maintainer carried it as #1920 with a guard for an output unplugged mid-request, merged by 2026-11-08 [docs] <https://github.com/cprussin/domicile/issues/1917> <https://github.com/cprussin/domicile/pull/1918> <https://github.com/cprussin/domicile/pull/1920>

## Where it touches Domicile

- `compositor` resolves the optional output resource to its advertised name and emits the typed fullscreen window request. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/xdg_shell.rs#L276) [source]
- `sdk` delivers window requests as a stream, and `manganese` subscribes until disposal. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-requests.ts#L12) [source]
- `manganese` carries the requested name into its reducer and tolerates an unplugged target. [#1920](https://github.com/cprussin/domicile/pull/1920) [docs]
- Status at the pin: landed. [#1920](https://github.com/cprussin/domicile/pull/1920) closes the display-choice behavior raised in [#1917](https://github.com/cprussin/domicile/issues/1917). [docs]

- The documented window-request stream can include a display with a fullscreen request; Manganese places the requesting window accordingly. [docs] [Shell request contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L487)

## Lineage

- 2026-11-07: the 2026-11-07 study isolates a shell-level field propagation issue from client-side missing requests. S-80a34a7f0718 · 2026-11-07 · Fullscreen output routing study [docs]
- 2026-11-09: the pinned compositor preserves the optional output name and the merged shell change handles it. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/xdg_shell.rs#L276) [source]

## Sources

- S-90a8684915e7 · 2026-11-09 · Sway native toplevel and popup request handling.

- S-3dc4b8b29e37 · 2026-11-09 · Scroll native toplevel and popup request handling.

- S-e77ffff40807 · 2026-11-09 · Native-window requests and shell placement contract.

- S-80a34a7f0718 · 2026-11-07 · Fullscreen output routing study.
- S-811bf6978f5a · 2026-11-09 · Merged fullscreen display request implementation record.
- S-97f867b6b31f · 2026-11-09 · Manganese window-request stream subscription.
- S-f5d75cc3208b · 2026-11-09 · Xdg fullscreen output and activation request routing.
