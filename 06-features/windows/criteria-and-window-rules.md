---
id: windows/criteria-and-window-rules
title: Window criteria and opening rules
kind: feature
family: windows
layers: [manganese, any-shell, sdk]
domicile_status: absent
domicile_refs: [packages/manganese-window-management/docs/KEYS.md, packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, pinnacle, jay, hyprland, niri]
eras: []
rows: [SW-0186, SW-0187, SW-0214, SW-0227, SW-0250, SW-0251, SW-0252, SW-0253, SW-0254, SW-0255, SW-0256, SW-0257, SW-0258, SW-0259, SW-0260, SW-0261, SW-0262, SW-0263, SW-0264, SW-0265, SW-0266, SW-0267, SW-0268, SW-0269, SW-0270, SW-0548, SW-0549, SW-0550, SW-0551, SW-1038, SW-1255, SW-1256, SW-1257, SW-1258, SW-1259, SW-1260, SW-1261, SW-1262, SW-1263, SW-1264, SW-1265, SW-1266, SW-1267, SW-1268, SW-1269, SW-1270, SW-1271, SW-1272, SW-1273, SW-1274]
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: false
bucket: domicile
tags: [family:windows, layer:manganese, status:absent, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-04a9199023c5, S-1712fb8a60e1, S-204a969601d0, S-23c521755f22, S-3025ab8ea45b, S-515bf507459b, S-56622eb226ce, S-65d1c77e9380, S-6792066180dd, S-759448a412d0, S-792ba21b119c, S-83c688ce4204, S-878e8ba58ca8, S-8c438f028a0f, S-8e001f804b21, S-90a8684915e7, S-92faa6ff2e46, S-9fe5a8a3f191, S-b1de89f2e134, S-c129e83bd9f7, S-d1e267110753, S-d5b15decb594, S-e33fe2e31922, S-e77ffff40807, S-f833707fdfaf]
---
# Window criteria and opening rules

Criteria select windows by attributes, and rules apply actions when matching windows appear or change relevant properties.

## Behavior

- Multiple attributes combine as a conjunction; regular expressions select titles, app identities and marks.
- A command list can retain a criterion across comma-separated operations and clear it at a semicolon.
- Opening rules and later attribute changes have explicit evaluation and repetition rules.
- Rule matching and the action applied to the match are separate contracts.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| sway @ pinned source | Criteria plus for_window and assign | Rule execution is tracked per view; title and app-ID changes can trigger newly matching rules. | [Criteria](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L977) · [Rule execution](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/view.c#L569) |
| scroll @ pinned source | Inherited criteria and command execution | The layout changes while the criteria/rule interface remains available. | [Window rules](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1629) |
| Pinnacle @ v0.2.4 | Configure-gated imperative opening rules | The initial configure waits for the new-window rule closure to finish. [docs] | [Release reference](https://github.com/pinnacle-comp/pinnacle/blob/v0.2.4/api/rust/src/window.rs#L923) |
| Jay @ v1.15.0 | Reactive window rules | Property changes re-evaluate matches, with entry/exit actions and a separate synchronous initial-placement phase. [docs] | [Release reference](https://github.com/mahkoh/jay/blob/v1.15.0/book/src/window-rules.md#L249) · [Release reference](https://github.com/mahkoh/jay/blob/v1.15.0/book/src/window-rules.md#L412) |
| Hyprland @ pinned source | Initial/current metadata and affected dynamic rules | Mapped, visible windows reset associated overrides and re-evaluate the affected rules as metadata changes. | [Matching fields](https://github.com/hyprwm/Hyprland/blob/151b0434114fad482e4fed9438c081b46e73423c/src/desktop/rule/windowRule/WindowRule.cpp#L383) · [Dynamic update](https://github.com/hyprwm/Hyprland/blob/151b0434114fad482e4fed9438c081b46e73423c/src/desktop/rule/windowRule/WindowRuleApplicator.cpp#L604) |
| niri @ pinned source | Ordered rules with opening and continuous properties | Alternative matchers use OR, fields use AND and exclusions veto; is-floating matching starts after opening. | [Rule combination](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Configuration:-Window-Rules.md#L133) · [Opening boundary](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Configuration:-Window-Rules.md#L255) |

## What the notes found

- Checked 2026-11-09: The pin’s command guide says general `for_window` rules are absent, while its layout guide documents app/title floating rules. The latter runs at the client’s first draw. Those claims describe different coverage levels. [source] [Command differences](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L160) · [Opening floating](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L204)
- Checked 2026-11-09: A process can supply more than one window, so a process identifier alone is not a unique window match. [docs] [Public matching discussion](https://github.com/dawsers/scroll/issues/339)
- Checked 2026-11-09: Sway rechecks xdg window rules after title or app-ID changes, skipping rule identities already recorded before their commands run. An error does not remove that record; unmap clears it. Manganese documents its app/title floating checks at first draw. [source] [Rule lifecycle](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/xdg_shell.c#L351) · [Rule lifecycle](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/view.c#L569) · [Rule lifecycle](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/view.c#L971) · [First-draw matching](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L204)
- Checked 2026-11-09: Manganese evaluates its opening app/title floating rules on the first native appearance. Later title and app-ID diffs update the record independently of that initial placement decision. [source] [Opening-rule lifecycle](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/floating/floating-rules.ts#L37) · [Opening-rule lifecycle](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-changes.ts#L46) · [Opening-rule lifecycle](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1311)
- Checked 2026-11-09: Sway command criteria can include non-view containers when con_mark or con_id is specified. The for_window rule path uses a separate per-view matcher. [source] [Per-view rule matcher](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/criteria.c#L483) · [Command collector](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/criteria.c#L500)
- 2026-11-04: Domicile docs state that per-window rules (a for_window equivalent) are not offered and that every window opens tiled [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L164>
- 2026-11-09: sway's assign command takes criteria plus a workspace or an output, and is documented as equivalent to a for_window rule that moves the container to that workspace or output (sway@1652c54) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L421>
- 2026-06-11: Rule evaluation uses window identity and state; geometry constraints, fullscreen state and capture exclusion remain separate consequences of a match (Chromium historical studies) [docs]

## Where it touches Domicile

- `manganese`: regular-expression app/title rules cover opening floating, and parented or fixed-size windows also open floating. [source] [Rule scope and timing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L204)
- `sdk`: The window list exposes the identity and metadata that a shell can inspect when applying its rules. [source] [Window read model](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L368)
- `any-shell`: The shell places each client by laying out its `<app>` element. [source] [Placement ownership](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L50)
- Status: `absent` for the general donor criteria/action language, with the documented opening-floating subset present. The absence search is the pinned command table and its explicit differences list. [source] [Differences](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L160)

## Lineage

- 2026-11-09 comparison at fixed donor revisions: the inventory separates criteria syntax, rule evaluation timing and the commands that rules execute. [source] [sway criteria](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L977)

## Sources

- S-9fe5a8a3f191 · 2026-09-21 · [Container commands and compositor configuration](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L1).
- S-204a969601d0 · 2026-11-06 · sway and Scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · [Window command vocabulary and default bindings](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L1).
- S-23c521755f22 · 2026-11-09 · [Grouped layouts, workspace homes and interaction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L1).
- S-e77ffff40807 · 2026-11-09 · [Shell entry, client elements and native-window control](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L1).
- S-3025ab8ea45b · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-f833707fdfaf · 2026-06-27 · Pinnacle release API documentation.
- S-04a9199023c5 · 2026-09-28 · Jay release documentation.
- S-c129e83bd9f7 · 2026-11-08 · Hyprland layout, rules and focus source at the donor pin.
- S-878e8ba58ca8 · 2026-11-08 · Hyprland layout, rules and focus source at the donor pin.
- S-1712fb8a60e1 · 2026-11-08 · Niri window-rule contract at the donor revision.
- S-515bf507459b · 2026-09-21 · [Native view lifecycle and matching rules](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/view.c#L1).
- S-90a8684915e7 · 2026-09-21 · [Native title and application-identity changes](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/xdg_shell.c#L1).
- S-b1de89f2e134 · 2026-11-09 · [Initial floating-rule expressions](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/floating/floating-rules.ts#L1).
- S-6792066180dd · 2026-11-09 · [Native appearance and property differences](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-changes.ts#L1).
- S-8c438f028a0f · 2026-11-09 · [Desk-wide window reducer and workspace policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L1).
- S-d1e267110753 · 2026-09-21 · [Root traversal and literal mark lookup](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/tree/root.c#L1).
- S-759448a412d0 · 2026-09-21 · [Command selectors and per-view matching](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/criteria.c#L1).
