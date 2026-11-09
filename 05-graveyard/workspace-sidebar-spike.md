---
id: graveyard/workspace-sidebar-spike
title: A workspace sidebar beside sway
kind: graveyard
layers: ["any-shell", "protocol", "external"]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
bucket: domicile
evidence_mix: mostly-source
sources: ["S-85883d471d8c", "S-71e61bbecc5d", "S-9e01e26cdfec", "S-b81011e5c1e1", "S-6088d82f56b9", "S-fbf47233dd83", "S-20ee76f17b56", "S-1fc047dd9001"]
tags: ["bucket:domicile", "evidence:docs", "donor:sway"]
---

# A workspace sidebar beside sway

A 2026-10-26..11-03 spike kept applications in Chrome while a separate native panel sat beside sway. Later reconstructions compared native hosts through the same bounded presentation and interaction contract. [docs]

## What it established

- A passive layer surface reserved desktop space while shown and released it when hidden. Ordinary-window previews were a separate mode. [docs]
- Native pointer input exercised selection, scrolling, local menus and section state. These actions changed local state; they did not establish live workspace or application integration. [docs]
- The GPUiX host lacked a useful resident hide operation. A supervisor retained selected state and replaced the renderer process on show and hide. [docs]
- A Rust/GPUI host instead kept the daemon alive while hiding its last native surface. Its ordinary-window preview reserved no desktop space. [docs]
- A GJS/GTK reconstruction used native controls through JSX without using React or a browser engine. [docs]
- A 2026-10-26 integration combined the sidebar, launcher and notification surfaces in one native host, with captures on one software-rendered output. Real-seat input, desktop-bus senders, clipboard use, multiple outputs and application launch remained unverified. [docs]

## Learnings

- Hiding a window, destroying a surface and replacing a renderer process produce different state and resource contracts. [docs]
- A recreated native renderer lost scroll, menu and collapsed-section state unless the supervisor retained them. [docs]
- Reserved space and ordinary-window preview behavior required separate qualification. [docs]
- Short isolated resource observations did not establish presentation latency, battery use or complete accessibility. [docs]

## Ending and carry-forward

Work stopped for reasons outside this library's scope; the technical state at that point was a native sidebar and several bounded reconstructions, with output hotplug, composition input and complete live integration unqualified. [docs]

The retained concepts connect to [panel surfaces](../06-features/shell-surfaces/layer-shell-equivalents.md) and [shell reload and recovery](../06-features/session/shell-reload-and-recovery.md).

## Sources

- S-85883d471d8c · 2026-11-02 · Native sidebar reconstruction and lifetime comparison.
- S-71e61bbecc5d · 2026-11-02 · Native React host behavior and state retention limits.
- S-9e01e26cdfec · 2026-10-26 · Notification contract study.
- S-b81011e5c1e1 · 2026-10-26 · Launcher expansion and native process-lifetime study.
- S-6088d82f56b9 · 2026-10-26 · Initial native sidebar integration account.
- S-fbf47233dd83 · 2026-10-26 · Combined native surfaces and explicit qualification limits.
- S-20ee76f17b56 · 2026-11-02 · Resident native process and distinct panel/preview surface modes.
- S-1fc047dd9001 · 2026-11-02 · Native GTK controls and the GJS/JSX authoring boundary.
