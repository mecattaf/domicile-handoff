---
id: build-and-packaging/nix-and-engine-builds
title: Nix engine artifacts and build identity
kind: feature
family: build-and-packaging
layers: [session, config]
domicile_status: landed
checked_at: "2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc"
answered_upstream: false
scroll_parity: n/a
donors: []
evidence_mix: mixed
boundary_note: false
bucket: domicile
tags: [family:build-and-packaging, evidence:source, bucket:domicile]
sources: [S-052e1d794319, S-064ec1298ca5, S-069533fa6b43, S-0d9cbb3168f0, S-24f0ff5009b7, S-328370bdeca1, S-37592c137fc7, S-37a0c1614d53, S-4ed3510bbb79, S-5ac26707cdda, S-695f43c6be84, S-6d51bf619041, S-6ee2a1600602, S-866b9f3f829d, S-8e6bc92b0b12, S-93533ae169f0, S-a19b54e8b274, S-c40e302fa0da, S-cd74fa28a51b, S-d9f18c535e19, S-ec0011587fbd]
eras: [the July 2026 native-shell floor, views-shell]
rows: [AL-views-inventory-192]
depends_on: []
reported_on_domicile: []
domicile_refs: [cprussin/domicile#1934, cprussin/domicile#1882, docs/SHELL-PACKAGING.md, packages/domicile-engine/README.md, docs/SHELL-CONFIG.md]
---
# Nix engine artifacts and build identity

A fixed desktop revision pins an exact engine artifact. The artifact identity includes the engine source and carry, so an optimized binary cannot silently omit a patch present in the checked build. [source] ([Series selection](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/engine-pin.nix#L16))

## Behavior

- Immutable artifact URLs and fetch hashes are tied to the desktop revision. [source] ([Release record](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/engine-release.nix#L12))
- Checked and optimized builds share an artifact identity while retaining their build-mode differences. [source] ([Selection contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/engine-pin.nix#L3))

## How donors do it

| Donor @ pin | Mechanism | Notable behavior or limit | Link |
|---|---|---|---|
| Domicile @ desktop pin | A fixed-output engine fetch names an immutable release series and hash | Checked and optimized artifacts share a content identity before substitution | [Engine pin](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/engine-pin.nix#L16) |

## What the notes found

- The October client-shell study distinguished its development build, release artifact, copied-source manifest and per-release carry. Its packaging expectations were design notes, not measurements of Domicile. [docs] S-064ec1298ca5.
- 2026-11-09: The Nix configuration example places the shell-module path under an executable-wrapper output, while the derivation defines the module in a separate page output [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-CONFIG.md#L85> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/flake.nix#L297> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/flake.nix#L558> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/flake.nix#L798> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/flake.nix#L889>
- 2026-11-05: The flake fetches a prebuilt engine named in the engine release file, so pinning source with engine patches does not compile them; fork pull requests need run approval and automatic repinning writes only to same-repository branches [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/flake.nix#L163> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/.github/workflows/engine.yml>
- 2026-11-04: The Nix docs name the page path `${manganese}/shell.js` (at the pin); a listing of the built manganese output at a 2026-11-04 release showed only bin/manganese, with shell.js in a separate page output [measured] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/INSTALL.md#L87> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-CONFIG.md#L85>
- 2026-11-07: Main of 2026-11-06 selected a checked engine (PROVENANCE: is_official_build false, dcheck_always_on true); by 2026-11-07 PR 1905 pointed the flake at an official engine build with checks off. [measured] <https://github.com/cprussin/domicile/pull/1905> <https://github.com/cprussin/domicile/pull/1893>
- 2026-11-04: Home Manager programs.domicile writes its own `domicile.json`, and two config files in the config folder are a hard stop; a NixOS module for binary and login beside a plain `domicile.tsx` avoids the collision [measured]
- 2026-11-04: A checked component build of the engine at the Chromium 159.0.8137.0 pin with patches 0169 and 0170 completed with its native tests passing; the upstream-documented cold-build footprint is 97 GB [measured]
- 2026-11-04: The effective browser flags of that engine build contained neither ThinLTO nor PGO, and a series-identity gate rejected the stock pin as not matching the combined patch series [measured]
- 2026-11-04: The engine packager attempted to read the locales directory as an ELF file; a regression test reproduces the error [measured]
- 2026-11-04: The documented alias settings.shell = "@domicile-desktop/manganese" resolves to the page (at the pin); the Nix example check compares strings and only evaluates, so the path mismatch passes the build [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/use/CONFIGURE.md#L36> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/INSTALL.md#L111>
- 2026-11-08: The tty VM checks attach a store image instead of reading the host store through 9p, separating engine startup from host file-sharing contention [docs] <https://github.com/cprussin/domicile/pull/1926>
- 2026-10-23: The native trial distinguished a repack of a separately compiled compositor binary from a hermetic Nix source derivation. No patched package activation was established by that bundle (Otto native trial packaging) [docs]
- 2026-11-04: The engine release packager recorded a fixed build host name, read a locales directory as an ELF header and warned on NUL bytes [docs]
- 2026-11-04: A packaged local engine tarball can be consumed through the flake's existing engine derivation by overriding its source, which keeps the NixOS library patching and launch wrapper intact [docs]
- 2026-11-07: A full engine source and build workspace held about 88 GB [docs]

## Where it touches Domicile

- The engine C ABI has its own shared-library target, including native alpha, crop, transform, frame, hold and submit helpers. The Chrome executable has a separate target. This establishes build membership; runtime loading and process ownership are separate facts. The [engine source comparison](../engine-mechanisms/gpu-and-render-backends.md) records the separate Rust loading path. [source] ([Native library](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/BUILD.gn#L4), [Chrome executable](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/BUILD.gn#L206))
- The [shell packaging contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-PACKAGING.md) covers the shell-facing package boundary. Engine artifact selection is implemented in the pinned Nix selector. [source] ([Selector](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/engine-pin.nix#L16))
- The historical Chromium-on-Nix work informs source/toolchain and artifact distinctions. It does not replace Domicile’s own engine release mechanism. [inferred]

- Status at the pinned checkout: `landed`. The source selects a fixed published engine artifact; the merged [engine pin update #1934](https://github.com/cprussin/domicile/pull/1934) and [engine package contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/README.md#L43) establish the release path. This does not establish a reproducible source rebuild. [docs]

- The pinned [engine wrapper includes the Vulkan loader in its runtime library path](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/flake.nix#L214). The [loader packaging change #1882](https://github.com/cprussin/domicile/pull/1882) merged before this pin. This establishes package wiring, leaving physical adapter and pixel qualification separate. [source]
- The pinned [configuration example](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-CONFIG.md#L85) selects an executable-wrapper output for its shell-module path, while the [derivation defines a separate page output](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/flake.nix#L798). The static example and output definitions therefore point at different package roots. [source]

## Lineage

- 2026-07-16: the native-shell floor kept Chromium compilation separate from its presentation-process package. [docs]
- 2026-10-01: the client-shell scope recorded release-mode packaging and per-milestone carry as separate records. [docs]

## Sources

- S-0d9cbb3168f0 · 2026-11-09 · Domicile published engine release record
- S-052e1d794319 · 2026-11-09 · Domicile engine artifact selector
- S-064ec1298ca5 · 2026-10-06 · Client-shell portability and release scope study
- S-37a0c1614d53 · 2026-11-09 · Domicile native engine library and executable build targets
- S-37592c137fc7 · 2026-11-09 · Domicile Nix wrapper dependencies and page outputs
- S-a19b54e8b274 · 2026-11-09 · Domicile merged Vulkan loader packaging change
- S-24f0ff5009b7 · 2026-11-09 · Domicile shell configuration example and module path
- S-6ee2a1600602 · 2026-11-09 pin check · Pinned shell packaging contract
- S-328370bdeca1 · 2026-11-09 pin check · Pinned engine packaging and dependency contract
