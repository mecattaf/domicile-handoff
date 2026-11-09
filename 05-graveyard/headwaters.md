---
id: 05-graveyard/headwaters
title: "Headwaters"
kind: graveyard
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-dfe38f275826, S-c6f41daa6909]
---

# Headwaters

A web shell in a native host exposed system data and operations through a typed namespace. [docs]

> Era and its assumptions: Headwaters, which assumed a native host supplied system capabilities to a typed web shell. For Domicile this probably adds little: Domicile already provides a page SDK and host interfaces. Recorded because serializable state, operations and subscriptions formed a durable contract.

## Dates

December 2025 ground truth, consolidated 2026-03-21. [docs]

## What it proved

The record preserves a concrete contract for data, operations and subscriptions; it does not supply a current publicly checkable runtime qualification. [docs]

## Key learnings

- Renderer framework and native transport were separate choices. [docs]
- Serializable state avoided exposing native resource lifetimes to the page. [docs]

## Ending

Work stopped for reasons outside this library's scope; the technical state at that point was a documented typed web-shell contract and reported native-host integration.

## What carried forward

The [typed seam](../01-chromium-lineage/mechanisms/typed-page-host-seam.md) passed into quickshellX and COWL.

## Sources

- S-dfe38f275826 · 2026-03-21 · Consolidated December web-shell contract
- S-c6f41daa6909 · 2026-07-03 · Content-dated spring shell chronology
