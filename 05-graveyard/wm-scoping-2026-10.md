---
id: graveyard/wm-scoping-2026-10
title: Window-manager scoping in October 2026
kind: graveyard
layers: ["any-shell", "compositor", "protocol"]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
bucket: domicile
evidence_mix: mostly-source
sources: ["S-d5410a77403b"]
tags: ["bucket:domicile", "evidence:docs", "donor:sway", "donor:scroll", "donor:niri"]
---

# Window-manager scoping in October 2026

The 2026-10-06 scoping compared scroll, niri and a standalone Chromium Views shell. It treated compositor semantics and shell implementation as separate decisions. The memo was a source comparison, not a completed desktop transition. [docs]

## What it established

- Scroll exposed configurable gesture bindings, in-process Lua and sway-derived seats; niri exposed a typed desktop event stream. The memo recorded different control and configuration contracts. [docs]
- A workspace rail could consume protocol or IPC state without requiring a particular client-rendering toolkit. The study corrected an earlier claim that scroll lacked the workspace protocol. [docs]
- The existing Views shell material included rendered and source-only parts. Layer-surface keyboard focus, current Chromium compatibility and multi-output behavior remained unresolved or conflicting. [docs]

## Learnings

- A donor command catalog did not establish a reusable desktop contract. [docs]
- Source presence, a rendered panel and a complete working shell were different evidence levels. [docs]
- Toolkit choice did not settle compositor policy or protocol coverage. [docs]
- A pin-coupled Lua interface and a typed external event stream carried different compatibility costs. [docs]

## Era and boundary

Era and its assumptions: October 2026 compositor scoping, which assumed a standalone native Views shell. For Domicile this probably adds little: Domicile already [places the shell in the compositor page](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md). Recorded because separating compositor semantics from shell policy remained useful. [docs]

## Ending and carry-forward

The memo closed as scoping material. Work stopped for reasons outside this library's scope; its technical state was a dated source comparison with explicit runtime uncertainties. It did not establish the untested configurations as working desktops. [docs]

The [sway family map](../04-sway-family/README.md) retains the command semantics.

## Sources

- S-d5410a77403b · 2026-10-06 · Compositor and native-shell scoping memo.
