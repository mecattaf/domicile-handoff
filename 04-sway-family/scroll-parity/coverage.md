---
id: sway-family/scroll-parity/coverage
title: Scope of the parity rows
kind: collection
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, bucket:domicile, evidence:source]
sources: [S-0e5ba5f15aa1, S-1fda0d64d596, S-204a969601d0, S-23c521755f22, S-2ee286174fbb, S-333f76cea05b, S-51a87fd37191, S-61be2f876d45, S-74a0a72c4129, S-935259f18cf1, S-95073f9e8efe, S-cae8be9e1e75, S-d5b15decb594, S-f30ab175558e, S-f735d75f3766]
---

# Scope of the parity rows

The [parity table](rows/scroll-parity.tsv) retains 2,006 distinct SW/SC identifiers. These rows include interfaces, option values, observed-state members, semantic invariants and explicit aliases. They are not a product-feature total or a runtime-conformance claim. [docs]

The source accounting maps 7,198 entries to these identifiers or records an explicit exclusion. It covers manuals, dispatch tables, argument literals, IPC members/events, Lua exports and callbacks, source test contracts, selected environment/CLI controls and document examples. No target identifier is missing from the public table. [docs] S-cae8be9e1e75 · 2026-11-06 · donor source-accounting inventory.

## Selected C interface

Nine selected header groups contribute 219 function declarations and 86 enum values. The accounting distinguishes 127 declarations mapped to existing behavior from 92 internal helpers; the enum values describe existing storage/options rather than 86 additional features. Each of these 305 records retains an existing SW/SC target. [docs] The declared tokens occur at their cited contexts in the current donor headers linked below. [source]

| Header group | Declarations and enum values in the accounting | Source |
|---|---:|---|
| Spatial layout | 154 | [pinned declaration group](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/include/sway/tree/layout.h#L1) |
| Animation | 57 | [pinned declaration group](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/include/sway/desktop/animation.h#L1) |
| Focus ring | 9 | [pinned declaration group](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/include/sway/tree/focus_ring.h#L1) |
| Saved spaces | 7 | [pinned declaration group](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/include/sway/tree/space.h#L1) |
| Output subdivisions | 24 | [pinned declaration group](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/include/sway/tree/virtual_output.h#L1) |
| Lua runtime | 13 | [pinned declaration group](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/include/sway/lua.h#L1) |
| Logging | 11 | [pinned declaration group](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/include/sway/log.h#L1) |
| Transactions | 12 | [pinned declaration group](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/include/sway/desktop/transaction.h#L1) |
| Command dispatch | 18 | [pinned declaration group](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/include/sway/commands.h#L1) |

Implementation members can be covered by a parent API row rather than a separate atomic field row. The source exposes fitting policy in both `workspace_get_mode` and `workspace_set_mode`; SC-0177 and SC-0178 carry those fields without additional identifiers. The catalog totals above do not assert that every implementation field was individually enumerated. [source] [Getter field](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L1456) · [Setter field](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L1529).

## Domicile dispositions

Current Domicile state is checked separately from donor coverage. A native protocol's registration and version establish that surface, not every member's behavior. Ordinary SDK field equivalents, partial tree/shell contracts and absent donor-specific surfaces therefore retain separate dispositions. Unverified rows remain explicit in the [family counts](../README.md).

The family counts also separate accounting aliases from behavioral rows. Added option values are classified independently of a changed parent command, and removed arbitrary-tree layout commands remain sway-only.

## Sources

- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-f735d75f3766 · 2026-11-06 · selected C interface accounting.
- S-cae8be9e1e75 · 2026-11-06 · donor source-accounting inventory.
- S-0e5ba5f15aa1 · 2026-11-05 · Scroll reference at the donor revision.
- S-1fda0d64d596 · 2026-11-05 · Scroll reference at the donor revision.
- S-23c521755f22 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-2ee286174fbb · 2026-11-05 · Scroll reference at the donor revision.
- S-51a87fd37191 · 2026-11-05 · Scroll reference at the donor revision.
- S-61be2f876d45 · 2026-11-05 · Scroll reference at the donor revision.
- S-74a0a72c4129 · 2026-11-05 · Scroll reference at the donor revision.
- S-935259f18cf1 · 2026-11-05 · Scroll reference at the donor revision.
- S-95073f9e8efe · 2026-11-05 · Scroll reference at the donor revision.
- S-d5b15decb594 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-f30ab175558e · 2026-11-05 · Scroll reference at the donor revision.
- S-333f76cea05b · 2026-11-05 · Scroll reference at the donor revision.
