---
id: 01-chromium-lineage/headwaters
title: "Headwaters: a web shell with a typed namespace"
kind: era
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-dfe38f275826, S-c6f41daa6909, S-1a653077d862]
---

# Headwaters: a web shell with a typed namespace

The December 2025 web-shell ground truth describes a transparent web interface in a native host, separated from system services by a typed namespace. The surviving consolidation is dated 2026-03-21; it is evidence of the contract and reported state, not a present runtime qualification. [docs]

## Dates and demonstrated scope

- The December ground truth names a native Qt/QML host, QtWebChannel bridge and SolidJS presentation tier. [docs]
- Its namespace separates window, workspace, audio, power and shell operations from the frontend's renderer. [docs]
- The consolidation's latency figures lack a checkable public experiment. [docs]

## Mechanisms and handoff

The transferable piece is the [typed page/host seam](mechanisms/typed-page-host-seam.md): serializable data, typed operations and subscriptions. Framework choice and transport are properties of the era. [inferred]

> Era and its assumptions: Headwaters, which assumed a separate native host supplied privileged capabilities. For Domicile this probably adds little: Domicile already provides a page-facing SDK and explicit host boundary. Recorded because the contract survived changes of renderer and native host.


## Where it touches Domicile

[The architecture](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ARCHITECTURE.md) describes Domicile's own seams. The historical namespace is recorded as a closed chapter. [docs]


## Sources

- S-dfe38f275826 · 2026-03-21 · Consolidated December web-shell contract
- S-c6f41daa6909 · 2026-07-03 · Content-dated spring shell chronology
- S-1a653077d862 · 2026-11-09 pin check · Pinned Domicile architecture and process boundary
