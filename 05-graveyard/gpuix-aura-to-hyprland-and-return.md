---
id: graveyard/gpuix-aura-to-hyprland-and-return
title: GPUiX and the return to sway
kind: graveyard
layers: ["any-shell", "protocol", "external"]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
bucket: domicile
evidence_mix: mostly-source
sources: ["S-beab1c3f96bb", "S-1773285f9c33", "S-71e61bbecc5d", "S-9840f843a070"]
tags: ["bucket:domicile", "evidence:docs", "donor:sway", "donor:hyprland"]
---

# GPUiX and the return to sway

The 2026-10-13..22 investigation explored a native React/Solid shell over a wlroots compositor. A brief Hyprland target gave way to sway. GPUIX supplied native retained nodes through its addon rather than an embedded browser page. [docs]

## What it established

- Ordinary native client windows rendered under an isolated sway compositor, with bounded search, selection and launcher interactions. [docs]
- Later shell probes established static panel reservation and more than one short-lived renderer in a process. They did not establish durable output selection, window lifetime control or a complete desktop shell. [docs]

## Learnings

- React or Solid authoring did not imply a DOM or web-engine rendering path. [docs]
- A native panel required explicit compositor protocols for reserved space, output routing and surface lifetime. [docs]
- Successful rendering and input were separate from complete shell services and physical-device qualification. [docs]
- Native controls required a host-specific behavior surface; browser CSS and DOM components did not transfer directly. [docs]
- A retained native tree and a compositor buffer had separate animation boundaries: client widget motion differed from whole-window motion. [docs]

## Ending and carry-forward

The Hyprland target ended with a return to sway. The full native shell did not reach activation in the preserved closure. Work stopped for reasons outside this library's scope; the technical state was working bounded native clients and incomplete shell integration. [docs]

The later [workspace sidebar spike](workspace-sidebar-spike.md) retained this distinction between view rendering, compositor integration and process lifetime.

## Sources

- S-beab1c3f96bb · 2026-10-21 · Native renderer history and Linux shell capability audit.
- S-1773285f9c33 · 2026-10-21 · Native-shell investigation closure.
- S-71e61bbecc5d · 2026-11-02 · Later native host lifetime study.
- S-9840f843a070 · 2026-10-23 · Follow-up native renderer and compositor boundary audit.
