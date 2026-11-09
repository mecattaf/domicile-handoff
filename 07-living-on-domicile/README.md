---
id: living-on-domicile
title: Observable symptoms on Domicile
kind: living
layers: [engine:input, engine:aura, engine:blink, engine:browser, engine:viz, engine:drm, compositor, protocol, config, sdk, manganese, system-apps, any-shell, host, session, external]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
evidence_mix: mixed
bucket: domicile
sources: [S-16bf250a5f9c, S-1a653077d862, S-24f0ff5009b7, S-24f31085af99, S-2ee24d100a88, S-37592c137fc7, S-4872dd803781, S-537297eb1290, S-5716f5a671cf, S-5d60bfe102cf, S-5f315b39ac7c, S-749b21ddc68b, S-74f557738f56, S-7cd85cb7ef61, S-8100b35ed50a, S-a886bb84a90d, S-a90d2490bf23, S-a92c840fc324, S-bfec45af03af, S-c2596d00b100, S-ca5a730a3a56, S-cc4c17362ba5, S-cf898850da22, S-e5e4d7fe8e6a, S-e8172659e482, S-ef6c7832a511, S-fd0c87388c02, S-fd20eb1c6dd6]
tags: [bucket:domicile, evidence:docs]
---

# Observable symptoms on Domicile

The [symptom table](rows/living.tsv) separates an observed or documented behavior from its disposition at the pinned upstream revision. A landed change records source and public change history; it does not claim a new physical-device retest. [docs]

Rows preserve the trigger and visible effect in hardware-neutral terms. Similar reports share one row. Historical symptoms remain visible after an upstream change settles them, with the corresponding change linked. [docs]

Dates identify the earliest retained report or the pinned documentation checkpoint for a source-only row. A behavior may predate that checkpoint.

## Reading status

- `landed`: a merged public change and corresponding pinned documentation establish the contract.
- `known-gap`: the pinned gap list describes the remaining behavior.
- `in-review`: a public change is open but outside the pinned revision.
- `by-design-elsewhere`: Domicile documentation assigns the case to another mechanism.
- `unknown`: a public source establishes the behavior or historical report, while the full disposition remains unverified.

The table records source-backed dispositions at the pin. It does not claim a complete physical-device qualification.

## Sources

- [Pinned known gaps](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md).
- S-8100b35ed50a · 2026-11-09 · Pinned documented gaps and desktop boundaries.
- S-e5e4d7fe8e6a · 2026-11-08 · Consolidated desktop symptom notes; neutral, externally checkable symptoms only.
- S-fd0c87388c02 · 2026-11-09 · Recent public Domicile changes.
- S-cc4c17362ba5 · 2026-11-09 · Pinned URL-opening policy.
- S-ca5a730a3a56 · 2026-11-09 · Pinned input observation guidance.
- S-749b21ddc68b · 2026-11-09 · Pairing callback boundary in the system Bluetooth service.
- S-16bf250a5f9c · 2026-11-09 · New Wi-Fi profile authentication in the NetworkManager backend.
- S-24f0ff5009b7 · 2026-11-09 · Pinned configuration defaults, input settings and reload boundaries.
- S-a90d2490bf23 · 2026-11-09 · Pinned gesture delivery and shell matching contract.
- S-e8172659e482 · 2026-11-09 · Launcher query and search-choice policy.
- S-537297eb1290 · 2026-11-09 · Keybinding ownership and shell-load recovery boundary.
- S-4872dd803781 · 2026-11-09 · SDK canonical chord and duplicate-binding contract.
- S-a886bb84a90d · 2026-11-09 · Desktop-entry terminal-wrapper exclusion.
- S-fd20eb1c6dd6 · 2026-11-09 · MIME-handler terminal-wrapper exclusion.
- S-2ee24d100a88 · 2026-11-09 · Independent component supervision and native-client connection lifetime.
- S-c2596d00b100 · 2026-11-09 · Portal capture and direct native-toplevel scope.
- S-74f557738f56 · 2026-11-09 · Pinned display profile and per-output gamma documentation.
- S-7cd85cb7ef61 · 2026-11-09 · Named night-light state and global reset semantics.
- S-cf898850da22 · 2026-11-09 · Manganese all-output temperature caller.
- S-5f315b39ac7c · 2026-11-09 · Remote desktop stream-coordinate calls and device grants.
- S-5716f5a671cf · 2026-11-09 · Separate screen-cast session handle validation.
- S-ef6c7832a511 · 2026-11-09 · Wayland capture readiness timestamps.
- S-24f31085af99 · 2026-11-09 · Captured frame metadata and native-window buffer paths.
- S-bfec45af03af · 2026-11-09 · Native capture CPU formats and optional GPU conversion.
- S-5d60bfe102cf · 2026-11-09 · Captured native-buffer representation.
- S-a92c840fc324 · 2026-11-09 · Native-window finger source, axis stops and synthetic momentum filtering.
- S-37592c137fc7 · 2026-11-09 · Pinned Nix desktop wrapper and browser-page output definitions.
- S-1a653077d862 · 2026-11-09 · Pinned application catalog and system-service adapter roles.
