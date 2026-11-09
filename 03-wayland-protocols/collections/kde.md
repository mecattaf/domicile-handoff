---
id: protocols/collections/kde
title: kde protocol collection
kind: collection
family: protocols
layers: [compositor, protocol, external]
domicile_status: n/a
domicile_refs: []
checked_at: "2026-11-09"
scroll_parity: baseline
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [layer:protocol, evidence:source, scroll-parity:baseline]
sources: [S-3acbfb4907e7, S-3f524db52413, S-4378aebc583f, S-43af2b3af183, S-8bf472405b20, S-a04ed2a57dc2, S-bad77f83f7f6, S-e7392f226f69, S-e9dc5003d109]
---

# kde protocol collection

KDE protocol compatibility surfaces. Collection names group this map; a collection label does not change the specification’s lifecycle or maturity. A dated family or module alias does not establish an exact wire version.

## At the pin

| Protocol | Purpose | Domicile | Evidence and limit |
|---|---|---|---|
| `KDE server-decoration` | Select client or server decoration through the KDE protocol | unknown; advertised, 1; server decoration mode | schema [scroll](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/protocol/server-decoration.xml) ; [source] [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L1276) ; version [source] [handler](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/shell/kde/decoration.rs#L121) ; status records unverified PR/documentation pairing; source advertisement/version is recorded independently |

## Lineage

| Protocol | Date | Documentary finding | Source |
|---|---|---|---|
| `KDE server-decoration` | 2026-10-06 | Named `org_kde_kwin_server_decoration` in a native client study. [docs] | S-e9dc5003d109 |
| `KDE server-decoration` | 2026-11-04 | A November source study cataloged this protocol family. [docs] | S-a04ed2a57dc2 |

## Sources

- [Complete coverage map](../coverage-map.md).
- S-3acbfb4907e7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-4378aebc583f · 2026-11-06 · Projected protocol members and schema coordinates.
- S-43af2b3af183 · 2026-11-09 · Protocol implementation and registration evidence.
- S-8bf472405b20 · 2026-11-09 · Protocol implementation and registration evidence.
- S-a04ed2a57dc2 · 2026-11-03 · November 4 source study of an earlier Domicile release.
- S-bad77f83f7f6 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e7392f226f69 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e9dc5003d109 · 2026-10-06 · Protocol implementation and registration evidence.
