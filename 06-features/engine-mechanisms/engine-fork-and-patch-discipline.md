---
id: engine-mechanisms/engine-fork-and-patch-discipline
title: Chromium fork and patch discipline
kind: feature
family: engine-mechanisms
layers: [engine:browser, session]
domicile_status: landed
checked_at: "2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc"
answered_upstream: false
scroll_parity: n/a
donors: []
evidence_mix: mixed
boundary_note: false
bucket: domicile
tags: [family:engine-mechanisms, evidence:source, bucket:domicile]
sources: [S-052e1d794319, S-0d9cbb3168f0, S-176943cfbcec, S-2fcac3a67284, S-328370bdeca1, S-332f2a79f6de, S-5056a8931e38, S-514bff6c69a0, S-5ac26707cdda, S-7194d751e6c4, S-7a209d6a376e, S-8e6bc92b0b12, S-93d683e28531, S-9f2616274b5c, S-9f8ae1b1c71c, S-a510e356465f, S-af786326abd1, S-c9366e4f4442, S-df0cfbeddc33, S-ec0011587fbd, S-ec3cb0925ecc, S-f36c86c29038]
eras: [the July 2026 native-shell floor, views-shell]
rows: [AL-cade-triage-019, AL-cade-triage-034, AL-cade-decision-006, AL-cade-decision-021, AL-cubs-coverage-008, AL-cubs-coverage-009, AL-cade-minimal-ash-006, AL-cade-fourth-verdicts-001, AL-wm-column-policy-026, AL-chromeos-extraction-001, AL-chromeos-extraction-002, AL-chromeos-extraction-003, AL-chromeos-extraction-004, AL-chromeos-extraction-005, AL-chromeos-extraction-006, AL-chromeos-extraction-007, AL-chromeos-extraction-008, AL-chromeos-extraction-009, AL-chromeos-extraction-010, AL-chromeos-extraction-011, AL-chromeos-extraction-012, AL-chromeos-extraction-013, AL-chromeos-extraction-014, AL-chromeos-extraction-015, AL-chromeos-extraction-016, AL-chromeos-extraction-017, AL-chromeos-extraction-018, AL-chromeos-extraction-019, AL-chromeos-extraction-020, AL-link-dep-scout-001, AL-link-dep-scout-002, AL-link-dep-scout-003, AL-link-dep-scout-004, AL-link-dep-scout-005, AL-link-dep-scout-006, AL-link-dep-scout-007, AL-link-dep-scout-008, AL-link-dep-scout-009, AL-link-dep-scout-010, AL-link-dep-scout-011, AL-link-dep-scout-012, AL-link-dep-scout-013, AL-flag-foundation-001, AL-flag-foundation-002, AL-flag-foundation-003, AL-flag-foundation-004, AL-flag-foundation-005, AL-flag-foundation-006, AL-flag-foundation-007, AL-flag-foundation-008, AL-flag-foundation-009, AL-flag-foundation-010, AL-flag-foundation-011, AL-flag-foundation-012, AL-flag-foundation-013, AL-igalia-patches-001, AL-igalia-patches-002, AL-igalia-patches-003, AL-igalia-patches-004, AL-igalia-patches-005, AL-igalia-patches-006, AL-igalia-patches-007, AL-igalia-patches-008, AL-igalia-patches-009, AL-protocol-prune-scout-008, AL-protocol-prune-scout-009, AL-protocol-prune-scout-012, AL-protocol-prune-deliberation-008, AL-protocol-prune-deliberation-011, AL-protocol-prune-deliberation-012, AL-protocol-prune-deliberation-013, AL-protocol-prune-deliberation-014, AL-drm-seat-resolve-025, AL-drm-seat-resolve-026, AL-drm-seat-resolve-027, AL-drm-seat-resolve-029, AL-drm-seat-resolve-030, AL-drm-seat-resolve-031, AL-drm-seat-resolve-032, AL-drm-seat-resolve-033, AL-drm-seat-resolve-034, AL-drm-seat-resolve-035, AL-drm-seat-resolve-036, AL-drm-seat-resolve-037, AL-drm-seat-resolve-038, AL-drm-seat-resolve-041, AL-drm-seat-resolve-042, AL-drm-seat-resolve-043, AL-drm-seat-resolve-045, AL-audio-design-006, AL-audio-design-007, AL-audio-design-008, AL-audio-design-009, AL-audio-design-010, AL-audio-design-011, AL-audio-design-012, AL-audio-design-017, AL-audio-design-018, AL-audio-design-026, AL-audio-design-029, AL-audio-design-030, AL-audio-design-032, AL-audio-design-033, AL-integration-review-047, AL-integration-review-048, AL-integration-review-049, AL-integration-review-050, AL-primary-selection-design-002, AL-primary-selection-design-006, AL-primary-selection-design-007, AL-primary-selection-design-016, AL-primary-selection-design-026, AL-protocol-prune-design-011, AL-protocol-prune-design-012, AL-protocol-prune-design-014, AL-protocol-prune-design-019, AL-protocol-prune-design-020, AL-protocol-prune-design-021, AL-protocol-prune-design-022, AL-protocol-prune-design-024, AL-drm-seat-design-001, AL-drm-seat-design-002, AL-drm-seat-design-003, AL-drm-seat-design-008, AL-drm-seat-design-023, AL-drm-seat-design-024, AL-drm-seat-design-025, AL-drm-seat-design-026, AL-drm-seat-design-027, AL-drm-seat-design-028, AL-drm-seat-design-029, AL-drm-seat-design-032, AL-drm-seat-design-033, AL-drm-seat-design-034, AL-brightness-design-001, AL-brightness-design-002, AL-brightness-design-003, AL-brightness-design-007, AL-brightness-design-009, AL-brightness-design-010, AL-brightness-design-011, AL-brightness-design-012, AL-brightness-design-014, AL-brightness-design-017, AL-brightness-design-019, AL-brightness-design-020, AL-brightness-design-021, AL-brightness-design-022, AL-brightness-design-023, AL-brightness-design-024, AL-brightness-design-025, AL-brightness-design-026, AL-brightness-design-027, AL-power-design-001, AL-power-design-003, AL-power-design-004, AL-power-design-005, AL-power-design-012, AL-power-design-013, AL-power-design-014, AL-power-design-015, AL-power-design-016, AL-power-design-017, AL-power-design-018, AL-power-design-019, AL-power-design-020, AL-power-design-021, AL-power-design-022, AL-power-design-023, AL-power-design-024, AL-power-design-025, AL-power-design-026, AL-power-design-027, AL-exo-target-dependencies-003, AL-exo-target-dependencies-005, AL-cade-june10-rendering-results-004, AL-cade-june10-rendering-results-019, AL-cade-june10-rendering-results-030, AL-cade-june10-rendering-results-032]
depends_on: []
reported_on_domicile: []
domicile_refs: [cprussin/domicile#1934, docs/architecture/ENGINE-FORK.md, packages/domicile-engine/README.md]
---
# Chromium fork and patch discipline

A fork has an upstream code revision, an overlay and patch set, and a built artifact identity. Those identities answer different reproducibility and maintenance questions. [source] ([Published engine identity](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/engine-release.nix#L16))

## Behavior

- The upstream reference and the exact published engine artifact have separate identities. [source] ([Immutable release](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/engine-release.nix#L12))
- Textual patch conflicts and API changes called by otherwise additive code are separate costs. [docs] S-5056a8931e38.

## How donors do it

| Donor @ pin | Mechanism | Notable behavior or limit | Link |
|---|---|---|---|
| Chromium study · 2026-07-16 | Separate new overlay files, build configuration and edits inside upstream functions | Textual merge conflicts and upstream API drift remain different costs | S-5056a8931e38 |
| Domicile @ desktop pin | A published engine has a content identity and fixed-output fetch hash | Optimized and checked artifacts are interchangeable only for the same series | [Engine selection](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/engine-pin.nix#L16) |

## What the notes found

- The 2026-07-16 appendix corrected a near-zero-cost claim for additive files: avoiding merge conflicts does not remove upstream API drift. Its small upstream-edit budget remained provisional while new hosting and presentation seams were unbuilt. [docs] S-5056a8931e38.
- 2026-11-06: Upstream checked engine hit a Blink container-query assertion ("0 vs. 2") from a small self-running HTML file with no compositor or native clients; the official engine completed the same file with exit 0 and no dump. The current official engine also completed it [measured] <https://github.com/cprussin/domicile/pull/1884>
- 2026-11-04: At runtime the compositor process mapped the selected engine shared library and the engine child ran from the selected package, giving a checkable identity for a locally built engine [measured]
- 2026-11-07: The engine's bootstrap helper scanned every Chromium source file to find changed submodules rather than only the submodule paths [measured]
- 2026-11-06: The DCHECK-enabled build aborted at shutdown on an assertion at partition_alloc_support.cc:764 after both typing measurements completed. The official build exited normally [measured]
- 2026-11-04: The repository guidance places input fixes in the engine when the engine owns the behavior; a preload-library approach was kept only as a comparison. Two new patches (0169, 0170) applied cleanly to the pinned series [measured]
- 2026-11-07: On the same long page, process CPU per accepted typed character was 80.5 ms in the checked engine and 24.0 ms in the official engine, about 3.35 times more, with identical engine-series identity and Chromium pin; DCHECK and PGO effects are combined [measured]
- 2026-11-07: Queued-input p95 during a long streamed page reply was about 111 ms in Chrome, 100 ms in the official engine and 522 ms in the checked engine, as instrumented software timings rather than physical key-to-pixel latency [measured]
- 2026-11-07: Contribution precedents on 2026-11-07: #1883 and #1884 preserved authorship and reconciled CI and patch numbering; #1904 reworked a GBM fix to reuse existing loading code with no new dependency. Rebasing an open PR on main movement restarts Engine CI [docs] <https://github.com/cprussin/domicile/pull/1883> <https://github.com/cprussin/domicile/pull/1884> <https://github.com/cprussin/domicile/pull/1904>
- 2026-11-04: Lifting Chromium's Exo out of Ash/Aura/Views was investigated and rejected for its dependencies; Domicile adopted an external Smithay server plus a Chromium surface bridge instead [docs] <https://github.com/cprussin/domicile/blob/08f0687dafa1018a9dcd7f227f43cc6dc557ffec/docs/architecture/STACKING-PARITY.md>
- 2026-11-07: Main began selecting checked engine builds (is_official_build=false, dcheck_always_on=true) with PROVENANCE recording that; a matched official build was not yet pinned, and release archives omit args.gn [docs] <https://github.com/cprussin/domicile/pull/1893>
- 2026-10-06: The standalone Views investigation separated compiled drawing from interaction and source-only browser features; common Skia ancestry did not establish compatible C++ runtimes, ABI or graphics-context ownership (chromium@154.0.8037.92) [docs]
- 2026-11-04: The engine at the Chromium 159.0.8137.0 pin was rebuilt as a checked component build with two additional patches in one ordered series; a series-identity gate rejected the stock pin [docs]

## Where it touches Domicile

- The [engine-fork rationale](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md) and its measurements are the current Domicile record at the pin. The historical patch inventory is not a Domicile rebase measurement. [docs]
- The pinned [artifact selector](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/engine-pin.nix#L16) chooses the optimized artifact only when its identity matches the checked artifact. [source]

Era and its assumptions: the July 2026 native-shell floor, which assumed a separate presentation process. For Domicile this probably adds little: Domicile already documents its engine fork and pins its published engine. Recorded because conflict-free overlay files can still incur API maintenance. [docs]

- Status at the pinned checkout: `landed` for the patch-series and published-engine mechanism. The [engine package](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/README.md#L16) records its ordered carry, and [the merged engine pin update #1934](https://github.com/cprussin/domicile/pull/1934) records artifact selection. Historical estimates remain separate from current rebase measurements. [docs]

- Rust loading checks connection and input-configuration symbols; descriptor polling and event dispatch resolve their own symbols separately, while frame-report support uses another symbol-presence check. [source] [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/engine.rs#L738), [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/engine.rs#L800), [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/engine.rs#L1652).

These source paths establish loading and recovery handling in the implementation; they do not establish an observed load, complete ABI compatibility or successful recovery. [inferred]

## Lineage

- 2026-07-16: the native-shell floor separated additive source, build configuration and upstream edits as maintenance axes. [docs]
- 2026-10-01: the client-shell census added copied-file manifests and per-milestone diffs to its portability notes. [docs]

## Sources

- S-5056a8931e38 · 2026-07-16 · Native-shell patch carry and unresolved boundaries study
- S-0d9cbb3168f0 · 2026-11-09 · Domicile published engine release record
- S-052e1d794319 · 2026-11-09 · Domicile engine artifact selector
- S-328370bdeca1 · 2026-11-09 pin check · Pinned engine packaging and dependency contract
- S-514bff6c69a0 · 2026-11-09 pin check · Pinned engine fork and patch contract
- S-a510e356465f · 2026-11-09 · Rust engine library, connection and session recovery implementation
