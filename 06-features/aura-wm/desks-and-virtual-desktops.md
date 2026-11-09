---
id: aura-wm/desks-and-virtual-desktops
title: "Desks and virtual desktops"
kind: feature
family: aura-wm
layers: [manganese, sdk, compositor]
domicile_status: unknown
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
answered_upstream: false
scroll_parity: n/a
donor_availability: chromeos-gated
donors: ["ash"]
evidence_mix: mixed
boundary_note: false
bucket: domicile
tags: [family:aura-wm, donor:ash, evidence:source, bucket:domicile]
sources: [S-b28649c0301e, S-82d30cf5a21b, S-af414fdb64d0, S-e4ff18df1f16]
eras: ["Aura lift (July 2026)", "views-shell"]
rows: ["AL-ash-desks-001", "AL-ash-desks-002", "AL-ash-desks-003", "AL-ash-desks-004", "AL-ash-desks-005", "AL-ash-desks-006", "AL-ash-desks-007", "AL-ash-desks-008", "AL-ash-desks-009", "AL-ash-desks-010", "AL-ash-desks-011", "AL-ash-desks-012", "AL-ash-desks-013", "AL-ash-desks-014", "AL-ash-desks-015", "AL-april-protocol-triage-030", "AL-april-protocol-triage-050", "AL-april-reverse-triage-008", "AL-tree-scout-004", "AL-rt-tree-findings-013", "AL-overview-desks-scout-001", "AL-overview-desks-scout-002", "AL-overview-desks-scout-006", "AL-overview-desks-scout-007", "AL-overview-desks-scout-008", "AL-overview-desks-scout-009", "AL-overview-desks-scout-010", "AL-overview-desks-scout-011", "AL-overview-desks-scout-012", "AL-overview-desks-scout-013", "AL-overview-desks-scout-014", "AL-overview-desks-scout-019", "AL-overview-desks-scout-024"]
depends_on: []
reported_on_domicile: []
domicile_refs: ["docs/architecture/ONE-PAGE-FOR-THE-DESK.md"]
---

# Desks and virtual desktops

A workspace groups windows, remembers activation state and defines how windows move between groups. Ash supplies a complete desk authority with explicit lifecycle and animation observers. [source] ([Desk contract](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/desks/desks_controller.h#L76))

## Behavior

- Lifecycle observers cover desk addition, removal, reordering, activation and naming. [source] ([Observers](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/desks/desks_controller.h#L76))
- Moving a minimized window restores it unless it is visible on all desks; shortcut movement can remove all-desk visibility. [source] ([Move edge cases](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/desks/desks_controller.h#L243))

## How donors do it

| Donor @ pin | Mechanism | Notable behavior or limit | Link |
|---|---|---|---|
| Ash @ Chromium 150 | `DesksController` creates, removes, reorders and activates desks | Activation selects the target desk’s MRU window; a move can name a different display root | [Desk operations](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/desks/desks_controller.h#L199) |


- Each ChromeOS-gated Ash display root receives a workspace controller for every desk container; inactive desk containers start hidden, with visibility animation enabled separately. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/root_window_controller.cc#L1073), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/root_window_controller.cc#L1190).

## What the notes found

- A 2026-10-01 study identified desk containers as part of Ash’s own window-tree authority. Mirroring a workspace model in another process does not preserve synchronous reparenting or layer mirrors. [docs] S-82d30cf5a21b.

## Where it touches Domicile

- The [one-page desktop contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md) places shell layout in the desktop page. Desk container ownership is a historical donor model, not a statement about the current shell implementation. [inferred]
- The comparable facts are membership, activation restoration, output scope and movement semantics; drawing a workspace list does not settle those facts. [inferred]

## Lineage

- 2026-07-18: the Aura study treated desk management as Ash-coupled logic. [docs]
- 2026-10-01: the client-shell study retained observer concepts and replaced desk mutation with compositor requests. [docs]

## Sources

- S-b28649c0301e · 2026-11-09 · Chromium desk lifecycle interface
- S-82d30cf5a21b · 2026-10-01 · Ash and client-shell window authority study
- S-af414fdb64d0 · 2026-11-09 · Ash state, display-root or preview implementation
- S-e4ff18df1f16 · 2026-11-09 pin check · Pinned one-page desktop design
