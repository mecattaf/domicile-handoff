---
id: "02-aura-ash-exo/README"
title: "Aura, Ash and Exo: source availability and equivalence"
kind: "collection"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile"]
sources: [S-4111d1fe471f, S-5d00de850efd, S-399343ab13db, S-fb25cecaef96, S-1f554a1713a5, S-9c1d22cd2b57, S-8100b35ed50a, S-8a920b088cbb, S-2a64ebf508b2, S-d1defc59c046, S-1a653077d862, S-514bff6c69a0]
---

# Aura, Ash and Exo: source availability and equivalence

**Exo is ChromeOS-gated.** Its [build target](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/BUILD.gn#L13) begins with `assert(is_chromeos)`. A file with no direct Ash references still inherits that gate. Domicile explicitly keeps its Rust compositor outside Exo and ports the useful buffer-import pattern into the browser. [source] [Domicile boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md#L352).

Ash has its own [ChromeOS build gate](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/BUILD.gn#L16). The [display-manager target](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/display/manager/BUILD.gn#L7) is gated too; generic `ui/display` is a different target. The [generic window-management target](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/BUILD.gn#L9) instead requires Aura and includes capture, cursor, focus and shadow machinery. Availability is a build fact before it is a coupling estimate. [source]

Domicile explicitly retains [Wayland, headless and DRM Ozone backends](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/scripts/build.sh#L66). Its downstream build changes permit DRM off ChromeOS. That retention does not remove the separate Ash, Exo or display-manager target gates. [source]

## Reading the map

The July map is a source survey, with later corrections applied. It records behavior and donor seams rather than a compiled transplant. Chromium source, target availability and Domicile status are separate columns and separate evidence. [docs]

The literal-verdict table contains 300 rows: `ADAPT` 99, `FRESH` 55, `LIFT` 146. Availability counts are `chromeos-gated` 197, `in-engine-build` 81, `rule-portable` 22. [docs]

The map retains 283 functional rows from the July verdict tables; the remaining source rows are exclusions, notes-only entries or optional design material. The October Chromium 154 inventory adds 17 functional mechanism rows. [docs]

The companion semantic table contains 1551 CADE/CUBS and coverage-study rows. Its design-coverage marks are kept outside `verdict_raw`: a drafted handler or a design marked covered does not become an implemented donor. [docs]

## Verdict crosswalk

| Historical mark | Reading in this library |
|---|---|
| LIFT or ADAPT | A reuse claim in its era; current `in-engine-build` or `rule-portable` requires a donor reference, and a gated target remains `chromeos-gated` |
| FRESH | The floor behavior is carried as an open implementation comparison; it is not an absence claim about Domicile |
| CORE or LATER | A shell or external responsibility in its dated context |
| IDEA | A shell pattern rather than an engine implementation |
| SCROLL | A compositor behavior in the scrolling-manager comparison |
| out | A stopped lineage item with its public reason retained |
| Tier and Exo percentage | Protocol-coverage input, not a summary of Domicile support |

## Twelve beats

| Beat | Literal rows | Historical verdicts |
|---|---:|---|
| [Aura window-management core](aura-wm-core.md) | 34 | ADAPT 10, FRESH 5, LIFT 19 |
| [Ash window states](ash-wm-states.md) | 24 | ADAPT 13, FRESH 4, LIFT 7 |
| [Ash desks](ash-desks.md) | 15 | ADAPT 5, FRESH 5, LIFT 5 |
| [Ash split view and window cycling](ash-splitview-cycle.md) | 14 | ADAPT 12, FRESH 2 |
| [Display management](display-mgmt.md) | 15 | ADAPT 8, FRESH 1, LIFT 6 |
| [Input and gestures](input-gestures.md) | 11 | ADAPT 2, FRESH 3, LIFT 6 |
| [Compositor blur and animation](compositor-blur-anim.md) | 19 | ADAPT 6, FRESH 5, LIFT 8 |
| [Ash system UI, shields and wallpaper](ash-sysui-blur-wallpaper.md) | 12 | ADAPT 3, LIFT 9 |
| [Exo core](exo-core.md) | 43 | ADAPT 8, FRESH 13, LIFT 22 |
| [Exo Wayland server and Ozone client](exo-wayland.md) | 55 | ADAPT 9, FRESH 13, LIFT 33 |
| [Views shell and strip behavior](views-shell-vtabs.md) | 19 | ADAPT 8, LIFT 11 |
| [Ash overview](ash-overview.md) | 22 | ADAPT 9, FRESH 4, LIFT 9 |

## Data and additional boundary

- [Literal lift rows](rows/aura-lift.tsv): component, historical verdict, coupling, donor availability, Domicile layer and canonical feature id.
- [CADE/CUBS semantic coverage](rows/cade-cubs-coverage.tsv): dated source disposition, qualification and feature mapping.
- [Current-engine checks](rows/engine-pin-checks.tsv): 20 donor groups checked at the engine pin, with source presence kept separate from target availability.
- [Capture-mode coverage](capture-mode-gap.md): the older inventory gap and the current capture boundary.

## Pins and verification limits

The donor study uses Chromium 150.0.7871.124. The October inventory explicitly uses Chromium 154.0.8037.92 and preserves `@154` component suffixes, despite the legacy table header. Domicile uses a later [Chromium base](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/CHROMIUM_PIN); the evaluated donor groups have fixed-engine source checks, including relocated strip controllers and changed shield styling. Historical verdicts are not rewritten as modern compile results. [docs]

Domicile status follows [the pinned architecture](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ARCHITECTURE.md), [known gaps](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md) and feature pages. No end-to-end performance equivalence follows from matching APIs. [inferred]

## Sources

- S-fb25cecaef96 · 2026-07-18 · Chromium lift-map summary
- S-1f554a1713a5 · 2026-07-18 · Lift-map evaluation and corrections
- S-9c1d22cd2b57 · 2026-10-01 · Chromium 154 shell-mechanism and component inventory
- S-4111d1fe471f · 2026-11-09 · Pinned Exo build gate
- S-5d00de850efd · 2026-11-09 · Pinned display-manager build gate
- S-399343ab13db · 2026-11-09 · Pinned generic window-management build target
- S-8100b35ed50a · 2026-11-09 pin check · Domicile rendering qualification boundaries
- S-8a920b088cbb · 2026-11-09 pin check · Pinned engine source-build entry point
- S-2a64ebf508b2 · 2026-11-09 pin check · Pinned Ash ChromeOS target gate
- S-d1defc59c046 · 2026-11-09 pin check · Pinned engine Chromium revision identity
- S-1a653077d862 · 2026-11-09 pin check · Pinned Domicile architecture and process boundary
- S-514bff6c69a0 · 2026-11-09 pin check · Domicile engine integration and patch discipline
