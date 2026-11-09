---
id: 01-chromium-lineage/views-shell-and-cowl-scheme
title: "views-shell and the cowl:// study"
kind: era
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-4d25f5ac98a2, S-80f21569978a, S-f86e13423d6e, S-fd16ec144654, S-c01d4f89b475, S-e77ffff40807, S-c7b0b84dd78c]
---

# views-shell and the cowl:// study

The October study separated a browser-free Views shell client from a web system page. The custom URL-scheme discussion addressed privileged browser integration; it was not itself evidence that a new compositor existed. [docs]

## Dates and demonstrated scope

- The November reconstruction dates the study to 2026-10-01–02 and describes a Views shell under an existing compositor. [docs]
- It distinguishes an earlier uncompiled Views design from later standalone shell work. The name alone does not identify a build or runtime result. [docs]
- A browser-hosted system page and a compositor shell surface have different authority. URL naming does not grant window placement, input focus or privileged host access. [inferred]

## System-page study

The October 1 source study separates a custom scheme from a system page. Its ordinary-page route uses an application window and a separately authorized native service; the scheme text itself grants no desktop authority. No route was installed or exercised in a live browser by that study. [docs]

## Later standalone result

A report dated 2026-10-02 records a browser-free Views program. Its documented build checks excluded non-data dependency paths to Content, V8, Chrome and the inspected Blink renderer targets. A separate dependency census still contained Blink public/common targets; target membership did not establish linked binary contents. It reports a layer-shell bar, a workspace command acknowledged by the external compositor, a menu as an xdg popup parented to the bar and a secondary text field receiving injected keyboard input after activation routing. All of this ran on a headless bench; no physical session ran the result. [docs]

## Handoff

The [typed seam](mechanisms/typed-page-host-seam.md) and [surface-role chapter](mechanisms/layer-shell-client-and-compositor.md) retain that distinction. [docs]

> Era and its assumptions: views-shell and the cowl:// study, which assumed a native client and a browser system page were separate products. For Domicile this probably adds little: Domicile already has a shell page with explicit SDK access. Recorded because the page origin and the surface role are separate authority boundaries.


## Where it touches Domicile

[Writing a shell](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md) defines how shell pages interact with Domicile at the pin. [docs]


## Sources

- S-fd16ec144654 · 2026-10-28 · Chromium and Aura lineage reconstruction
- S-c01d4f89b475 · 2026-11-04 · Earlier compositor ideas compared with Domicile
- S-80f21569978a · 2026-10-02 · Standalone Views build and headless interaction result
- S-f86e13423d6e · 2026-10-01 · Recovered shell attempts and evidence boundaries
- S-4d25f5ac98a2 · 2026-10-01 · Custom-scheme and stock-browser system-page source study
- S-e77ffff40807 · 2026-11-09 pin check · Pinned shell authoring contract
- S-c7b0b84dd78c · 2026-10-02 · Historical target graph, renderer exclusions and linked-code distinction
