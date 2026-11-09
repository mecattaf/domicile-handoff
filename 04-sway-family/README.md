---
id: sway-family
title: sway and scroll contracts
kind: collection
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
evidence_mix: mostly-source
bucket: domicile
tags: [donor:sway, donor:scroll, scroll-parity:baseline, bucket:domicile, evidence:source]
sources: [S-0c2272ae6a84, S-102d2919cad6, S-204a969601d0, S-23c521755f22, S-37f72d290d44, S-5a7999d26d19, S-5fc4682d5913, S-66f102a84e79, S-8bb658bd1300, S-8c438f028a0f, S-9ca9233a2f5e, S-9fe5a8a3f191, S-b57f8ac4d04a, S-c3cf234a5550, S-d34e89f64bf1, S-d5b15decb594, S-db9ca1904b13, S-f88b70fd0dfa]
---

# sway and scroll contracts

The mapped donor interfaces cover more behavior than the reusable mechanisms present at the pin. The counts below separate existing mechanisms from complete donor behavior; a partial family does not mean every row in it is implemented. Row totals count source interfaces, options, fields, semantics and explicit aliases, rather than product features. The totals are derived from the [public row table](scroll-parity/rows/scroll-parity.tsv). [docs]

Scroll parity is the baseline. Scroll substitutes a scrolling row/column model for sway's arbitrary split, tabbed and stacked layout model. It is not a strict command superset. [source] [Donor comparison](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/README.md#L2127).

## Mechanism counts

| Layer | Rows touched | Landed behavior | Partial contracts | Exact contract absent | Unverified | Aliases and exclusions |
|---|---:|---:|---:|---:|---:|---:|
| any-shell | 412 | 8 | 62 | 334 | 2 | 6 |
| compositor | 642 | 15 | 119 | 431 | 46 | 31 |
| config | 359 | 6 | 53 | 233 | 53 | 14 |
| engine:aura | 4 | 0 | 2 | 2 | 0 | 0 |
| engine:blink | 222 | 0 | 19 | 169 | 13 | 21 |
| engine:browser | 15 | 2 | 13 | 0 | 0 | 0 |
| engine:drm | 37 | 3 | 5 | 10 | 19 | 0 |
| engine:input | 94 | 0 | 51 | 43 | 0 | 0 |
| engine:viz | 11 | 1 | 2 | 0 | 8 | 0 |
| external | 148 | 0 | 1 | 131 | 8 | 8 |
| manganese | 826 | 12 | 81 | 699 | 9 | 25 |
| protocol | 104 | 1 | 47 | 42 | 14 | 0 |
| sdk | 952 | 21 | 137 | 748 | 10 | 36 |
| session | 17 | 0 | 4 | 2 | 11 | 0 |
| system-apps | 107 | 0 | 0 | 107 | 0 | 0 |

## Family map

Relations are same / scroll-changed / scroll-only / sway-only. Coverage is landed / partial / absent; unverified and accounting-alias rows are separate. Layers count row touches, so a row can occur in more than one layer.

| Family | Rows | Relations | Domicile coverage | Unverified / aliases and exclusions | Layer counts | Feature |
|---|---:|---|---|---|---|---|
| [Scrolling rows and columns](scroll-parity/scrolling-rows-and-columns.md) | 114 | 37 / 15 / 62 / 0 | 0 / 21 / 93 | 0 / 0 | any-shell 114 | [layout/scrolling-layout](../06-features/layout/scrolling-layout.md) |
| [Fit, align and center](scroll-parity/fit-align-and-center.md) | 48 | 1 / 0 / 47 / 0 | 0 / 0 / 48 | 0 / 0 | any-shell 48 | [layout/scrolling-layout](../06-features/layout/scrolling-layout.md) |
| [Jump labels](scroll-parity/jump.md) | 26 | 0 / 3 / 23 / 0 | 0 / 0 / 24 | 0 / 2 | any-shell 26 | [windows/marks-and-advanced-marking](../06-features/windows/marks-and-advanced-marking.md) |
| [Overview](scroll-parity/overview.md) | 19 | 0 / 0 / 19 / 0 | 0 / 5 / 11 | 0 / 3 | engine:blink 19; manganese 19 | [workspaces/overview](../06-features/workspaces/overview.md) |
| [Trails, selections and saved spaces](scroll-parity/trails-and-advanced-marking.md) | 50 | 0 / 0 / 50 / 0 | 0 / 0 / 50 | 0 / 0 | any-shell 50 | [windows/marks-and-advanced-marking](../06-features/windows/marks-and-advanced-marking.md) |
| [Virtual outputs](scroll-parity/virtual-outputs.md) | 12 | 0 / 1 / 11 / 0 | 0 / 0 / 12 | 0 / 0 | any-shell 12; compositor 12 | [workspaces/workspace-model-and-homes](../06-features/workspaces/workspace-model-and-homes.md) |
| [Marks](scroll-parity/marks.md) | 19 | 19 / 0 / 0 / 0 | 0 / 0 / 16 | 0 / 3 | any-shell 19 | [windows/marks-and-advanced-marking](../06-features/windows/marks-and-advanced-marking.md) |
| [Content scaling](scroll-parity/content-scaling.md) | 13 | 0 / 1 / 12 / 0 | 0 / 0 / 10 | 1 / 2 | compositor 13; engine:blink 13; sdk 1 | [windows/content-scaling](../06-features/windows/content-scaling.md) |
| [Animations and feel](scroll-parity/animations-and-feel.md) | 179 | 100 / 9 / 69 / 1 | 0 / 14 / 147 | 2 / 16 | compositor 1; engine:blink 179; manganese 179 | [feel-and-animation/scroll-feel-parity](../06-features/feel-and-animation/scroll-feel-parity.md) |
| [Continuous gesture scrolling](scroll-parity/continuous-gesture-scrolling.md) | 14 | 1 / 0 / 13 / 0 | 0 / 0 / 14 | 0 / 0 | any-shell 14; sdk 14 | [feel-and-animation/scroll-feel-parity](../06-features/feel-and-animation/scroll-feel-parity.md) |
| [Lua scripting](scroll-parity/lua-scripting.md) | 127 | 0 / 0 / 127 / 0 | 8 / 41 / 77 | 0 / 1 | any-shell 127; sdk 127 | [ipc-scripting/lua-and-shell-scriptability](../06-features/ipc-scripting/lua-and-shell-scriptability.md) |
| [IPC observations and events](scroll-parity/ipc.md) | 372 | 272 / 0 / 100 / 0 | 7 / 46 / 294 | 0 / 25 | compositor 372; sdk 372 | [ipc-scripting/read-model-and-events](../06-features/ipc-scripting/read-model-and-events.md) |
| [Criteria and window rules](scroll-parity/criteria-and-window-rules.md) | 50 | 50 / 0 / 0 / 0 | 0 / 4 / 46 | 0 / 0 | manganese 50; sdk 50 | [windows/criteria-and-window-rules](../06-features/windows/criteria-and-window-rules.md) |
| [Scratchpad and floating windows](scroll-parity/scratchpad-and-floating.md) | 20 | 14 / 1 / 5 / 0 | 1 / 4 / 15 | 0 / 0 | compositor 3; manganese 20; sdk 4 | [layout/floating-and-scratchpad](../06-features/layout/floating-and-scratchpad.md) |
| [Workspaces and output rules](scroll-parity/workspaces-and-outputs.md) | 134 | 121 / 5 / 8 / 0 | 6 / 9 / 92 | 24 / 3 | any-shell 2; compositor 14; config 127; engine:blink 2; engine:drm 28; engine:viz 6; external 3; manganese 92; protocol 4; sdk 1 | [workspaces/workspace-model-and-homes](../06-features/workspaces/workspace-model-and-homes.md) |
| [Seat and input configuration](scroll-parity/seat-and-input-config.md) | 137 | 137 / 0 / 0 / 0 | 0 / 57 / 54 | 23 / 3 | compositor 62; config 107; engine:aura 2; engine:blink 7; engine:browser 9; engine:drm 1; engine:input 89; manganese 2; protocol 34; sdk 18; session 8 | `seats/independent-seat` |
| [Idle and lock](scroll-parity/idle-and-lock.md) | 11 | 11 / 0 / 0 / 0 | 0 / 4 / 3 | 4 / 0 | compositor 10; engine:drm 1; engine:input 2; engine:viz 1; manganese 8; protocol 5; sdk 2; session 9 | `session/idle-and-lock` |
| [Bar, status and tray](scroll-parity/bar-and-status.md) | 92 | 89 / 0 / 3 / 0 | 0 / 5 / 86 | 0 / 1 | manganese 92; sdk 92 | [bar-and-notify/status-bar-and-tray](../06-features/bar-and-notify/status-bar-and-tray.md) |
| [Binding modes and shortcuts](scroll-parity/binding-modes.md) | 107 | 74 / 26 / 7 / 0 | 4 / 12 / 90 | 0 / 1 | compositor 1; engine:browser 6; manganese 106; protocol 1; sdk 106 | [bindings-and-modes/binding-modes](../06-features/bindings-and-modes/binding-modes.md) |
| [Tiling splits and containers](scroll-parity/tiling-splits-and-containers.md) | 31 | 0 / 2 / 0 / 29 | 0 / 11 / 20 | 0 / 0 | manganese 31 | [layout/tiling-splits-and-containers](../06-features/layout/tiling-splits-and-containers.md) |
| [Focus and urgency](scroll-parity/focus-and-urgency.md) | 79 | 52 / 10 / 17 / 0 | 2 / 8 / 68 | 0 / 1 | compositor 79; manganese 78; protocol 1 | [windows/focus-and-urgency](../06-features/windows/focus-and-urgency.md) |
| [Window state and fullscreen](scroll-parity/window-state-and-fullscreen.md) | 32 | 17 / 5 / 10 / 0 | 1 / 6 / 23 | 2 / 0 | compositor 5; manganese 32; sdk 32 | [layout/tiling-splits-and-containers](../06-features/layout/tiling-splits-and-containers.md) |
| [Client and protocol contracts](scroll-parity/client-and-protocol-contracts.md) | 69 | 65 / 0 / 4 / 0 | 2 / 31 / 22 | 14 / 0 | compositor 65; config 3; engine:aura 2; engine:blink 2; engine:drm 7; engine:input 3; engine:viz 3; external 6; manganese 6; protocol 58; sdk 6 | `protocols/coverage-map` |
| [Command syntax and process configuration](scroll-parity/command-and-process-config.md) | 144 | 133 / 4 / 7 / 0 | 0 / 4 / 129 | 3 / 8 | compositor 5; config 122; engine:viz 1; external 139; manganese 4; protocol 1; sdk 127 | [ipc-scripting/lua-and-shell-scriptability](../06-features/ipc-scripting/lua-and-shell-scriptability.md) |
| [Notification helper contracts](scroll-parity/notification-helper.md) | 107 | 90 / 4 / 7 / 6 | 0 / 0 / 107 | 0 / 0 | manganese 107; system-apps 107 | [bar-and-notify/notifications](../06-features/bar-and-notify/notifications.md) |

## Reading the rows

Pinned donor-file dates identify the revision, while dated studies and public comments retain their own dates. A revision date does not assign that introduction date to every behavior described.

The donor column records source contracts at fixed refs. `same` means a shared documented interface or native mechanism, with source qualifications; it does not assert runtime equivalence. Option atoms are classified independently of their parent command: added flags are scroll-only, changed release timing is scroll-changed, and removed arbitrary-tree commands are sway-only. Domicile status is checked against its current pin, independently of earlier matrix assessments. `partial` in count tables is a semantic comparison, while each row retains the controlled Domicile disposition.

The bounded command/state inventory search covers the complete Manganese command parser, state declarations, command guide and window-behavior guide. An absent disposition is limited to that surface and pin. Native protocol rows retain an unverified disposition until the native-source map supplies their evidence.

The earlier verdict vocabulary crosses over as follows: LIFT/ADAPT maps to in-engine-build or rule-portable with donor evidence; FRESH leaves the behavior open; CORE/LATER records any-shell or external mechanisms; IDEA records an any-shell pattern; SCROLL records compositor machinery; out records stopped lineage; tiered Exo percentages are protocol rows. These historical labels do not assign ownership in Domicile.

## Files

- [Scroll parity](scroll-parity/): family pages and the complete public row table.
- [sway semantics](sway-semantics.md): inherited command and state contracts.
- [swayfx](swayfx.md): effects and window-policy additions.
- [sway-next](sway-next.md): the October prototype and its technical state.
- [any1 branch](any1-branch.md): branch-specific behavior.
- [dawsers threads](dawsers-threads.md): public discussion evidence.
- [Glossary crosswalk](glossary-rows.tsv): terminology across donors.

## Sources

- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-5a7999d26d19 · 2026-11-06 · donor inventory closure and caveats.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.

- S-c3cf234a5550 · 2026-11-09 · Ash floated-window desk association at the engine pin.
- S-f88b70fd0dfa · 2026-11-09 · Ash owning-desk lookup at the engine pin.

- S-8c438f028a0f · 2026-11-09 · Manganese workspace homes and selection at the pin.

Glossary anchors: [sway containers](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L178), [Scroll rows and columns](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/TUTORIAL.md#L14), [Ash desk scope](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.h#L29), [Manganese groups and workspaces](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L6). [source]

Ash floated-window desk association: [desk association bookkeeping](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desk.cc#L830) · [owning-desk lookup](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/desks/desks_util.cc#L136). [source]

Manganese workspace homes: [empty hidden workspace exception](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L138) · [workspace selection fallback](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L2017). [source]

- S-102d2919cad6 · 2026-11-05 · Scroll reference at the donor revision.
- S-37f72d290d44 · 2026-11-05 · Scroll reference at the donor revision.
- S-9fe5a8a3f191 · 2026-09-21 · sway reference at the donor revision.
- S-d5b15decb594 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
