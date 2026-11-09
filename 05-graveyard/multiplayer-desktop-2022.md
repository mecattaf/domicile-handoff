---
id: graveyard/multiplayer-desktop-2022
title: A multiplayer desktop on sway and VNC
kind: graveyard
layers: ["compositor", "external"]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
bucket: domicile
evidence_mix: mostly-source
sources: ["S-40bc5a63b108"]
tags: ["bucket:domicile", "evidence:docs", "donor:sway"]
---

# A multiplayer desktop on sway and VNC

A 2022-23 desktop combined sway with VNC access. Its configuration associated each collaborator with a named sway seat and a corresponding VNC server. The preserved fork added container-level color rules. [docs]

## What it established

The configuration expressed a shared desktop with several cursors and focus contexts through named seats. Cursor themes identified seats, while window marks selected container colors. These were separate mechanisms. The surviving technical account records configuration and source history, rather than a current end-to-end qualification. [docs]

## Learnings

- Seat selection, remote transport and window ownership markings remain distinct contracts. [docs]
- A mark can trigger matching rules without creating another seat. [docs]
- Per-container colors required a fork; stock global color commands did not preserve that scope. [docs]
- Independent cursors did not make the shared compositor a security boundary. [inferred]

## Ending and carry-forward

Work stopped for reasons outside this library's scope; the technical state at that point was a sway/VNC desktop with per-seat configuration and a narrow color-rule fork. [docs]

Its technical lineage connects to [independent seats](../06-features/seats/independent-seat.md), [window marks](../06-features/windows/marks-and-advanced-marking.md) and [the any1 branch study](../04-sway-family/any1-branch.md). Later uses are outside this library's scope.

## Sources

- S-40bc5a63b108 · 2026-10-22 · Historical seat, VNC and container-rule source comparison; only repository and compositor-configuration facts are retained.
- [sway seat configuration reference](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-input.5.scd).
