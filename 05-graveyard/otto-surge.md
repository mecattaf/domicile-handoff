---
id: graveyard/otto-surge
title: The October Otto native-shell investigation
kind: graveyard
layers: ["compositor", "any-shell"]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
bucket: domicile
evidence_mix: mostly-source
sources: ["S-4bc64a8acde1", "S-7eefe75063cb", "S-411dca61faa5", "S-37c232e15627", "S-d31169455550", "S-9e34748315db", "S-74e1aab5679a", "S-fc2f55a1f3e8", "S-d62c301fab7c"]
tags: ["bucket:domicile", "evidence:docs", "donor:otto"]
---

# The October Otto native-shell investigation

A 2026-10-23..28 investigation examined Otto as a patched compositor base and as the host for a native shell. It combined a narrow React-to-Otto-kit adapter, backdrop-blur compatibility work and a separate laptop trial. [docs]

> Era and its assumptions: the October Otto investigation, which assumed a patched Otto compositor could host a native shell. For Domicile this probably adds little: Domicile already owns its compositor and renders the shell in a page. Recorded because a working initial frame, a blur adapter and virtual-terminal recovery were separate qualifications.

## What it established

- A shared React workload used either the existing GPUIX native host or a small Otto-kit host. Both retained native ASCII text echo while the React thread was blocked. The fixture did not cover composition input, a full toolkit or physical input latency. [docs]
- A compositor-side adapter translated KDE blur requests into Otto's existing backdrop machinery. Captures covered both a layer surface and an ordinary application window; they did not qualify frame pacing or hardware power. [docs]
- The stock Vulkan compositor initially displayed applications on an Intel laptop with two eDP panels. Returning to its virtual terminal later produced atomic display-configuration errors while the process remained alive. [docs]
- The local patch set also covered fixed output origins, migration of ordinary and minimized windows on output loss, modifier-drag and display reset on terminal return. The recorded resume patch had no hardware qualification. [docs]
- The October desktop concept on Otto remained a component and interaction mapping, distinct from the native-host and blur proofs. [docs]

## Learnings

- Native text echo and React state updates have distinct scheduling boundaries. [docs]
- A client renderer and a compositor renderer can interoperate through surface protocols without sharing a graphics backend. [docs]
- A working initial desktop frame does not establish virtual-terminal recovery. [docs]
- Shared Skia ancestry did not make Chromium Views a linkable Otto toolkit; a separate study retained narrow tab-policy arithmetic instead. [docs]
- GPU-capable rendering did not establish power or responsiveness advantages, and component pictures did not establish complete interaction or lifecycle. [docs]

## Ending and carry-forward

The physical trial stopped after display-state recovery could not be established. The broader native-shell investigation stopped for reasons outside this library's scope; its technical state was a narrow working host, a captured blur adapter and unqualified hardware integration. [docs]

The retained mechanisms connect to [backdrop blur](../06-features/effects/backdrop-blur.md), [window groups](../06-features/browser-windows/tab-groups-and-strips.md) and [shell lifecycle](../06-features/session/shell-reload-and-recovery.md).

## Sources

- S-4bc64a8acde1 · 2026-10-23 · Matched native React-host experiment.
- S-7eefe75063cb · 2026-10-23 · Compositor-side blur compatibility and captures.
- S-411dca61faa5 · 2026-10-23 · Physical startup and display-state recovery account.
- S-37c232e15627 · 2026-10-23 · Fixed-mode output topology qualification boundary.
- S-d31169455550 · 2026-10-06 · Views runtime and tab-policy portability study.
- S-9e34748315db · 2026-10-23 · Patched compositor-base architecture and qualification audit.
- S-74e1aab5679a · 2026-10-23 · Native desktop concept mapping and implementation boundary.
- S-fc2f55a1f3e8 · 2026-10-23 · Native component catalog evidence and lifecycle boundary.
- S-d62c301fab7c · 2026-10-23 · Version-qualified native toolkit and rendering comparison.
