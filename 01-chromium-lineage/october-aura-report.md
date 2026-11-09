---
id: 01-chromium-lineage/october-aura-report
title: "A 2026-10-28 Chromium/Aura lineage report"
kind: era
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-fd16ec144654, S-1a653077d862]
---

# A 2026-10-28 Chromium/Aura lineage report

The October report reconstructed the compositor lineage while preserving the difference between original constraints, contemporary runtime reports and later interpretation. [docs]

## What it established

- COWL's layer-shell client path and CADE's compositor path were alternatives, not successive releases of one completed desktop. [docs]
- The June VM compositor proof had a narrower boundary than production hardware qualification. [docs]
- Scene ownership enabled coordinated animation, mirrors and real-window manipulation; preserving those mechanisms did not settle which outer compositor or shell toolkit to use. [docs]
- A later architectural summary was not automatically an original decision or a runtime result. [docs]

## What it handed forward

This report supplies a chronology and evidence-boundary check. Each era page links its own mechanism rather than treating a later reconstruction as direct implementation evidence. [inferred]

## Where it touches Domicile

The report predates the November comparison. Domicile status at the checked pin follows [the pinned architecture](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ARCHITECTURE.md), not from that reconstruction. [docs]


## Sources

- S-fd16ec144654 · 2026-10-28 · Chromium and Aura lineage reconstruction
- S-1a653077d862 · 2026-11-09 pin check · Pinned Domicile architecture and process boundary
