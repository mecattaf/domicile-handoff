---
id: "02-aura-ash-exo/ash-overview"
title: "Ash overview"
kind: "collection"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile", "evidence:source"]
sources: [S-1f554a1713a5, S-3f3c8ab94769, S-4a6520490f9a, S-4ea478cdaf86, S-e19473fe102a]
---

# Ash overview

Ash overview transforms live windows and transient descendants, retains restoration state and mirrors minimized windows. The class remains ChromeOS-gated. [docs]

## What Chromium already has

The [row data](rows/aura-lift.tsv) holds 22 rows for this beat: ADAPT 9, FRESH 4, LIFT 9. These are historical source verdicts. [docs]

Donor availability in those rows: `chromeos-gated` 22. Zero Ash references in a file do not bypass its target build gate. [docs]

- Ash overview changes live window transforms and also references Ash shadow and split-view controllers; this is a mechanism reference rather than a directly available Linux overview controller. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L259), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/overview/scoped_overview_transform_window.cc#L487).
- ScopedOverviewTransformWindow manages a live window and transient descendants, restores prior state on destruction, and uses mirrored layers for minimized windows. [source] [Donor source](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/overview/scoped_overview_transform_window.h#L37), [Donor source](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ash/wm/overview/scoped_overview_transform_window.h#L121).

## Domicile at the pin

- `manganese`: The current overview transforms page elements and clips them to their monitor through a separate wrapper; a transition can reverse midway. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/overview/seen-styles.ts#L8).
- Public changes: [overview #1931](https://github.com/cprussin/domicile/pull/1931), [overview transition #1940](https://github.com/cprussin/domicile/pull/1940), [closing motion #1910](https://github.com/cprussin/domicile/pull/1910), [closing lifetime #1939](https://github.com/cprussin/domicile/pull/1939), and [workspace slide #1942](https://github.com/cprussin/domicile/pull/1942). [docs]

## Boundary

Both systems can transform a live scene. Restoration, transient membership, input targeting and minimized content remain separate invariants; resemblance does not prove implementation equivalence. [inferred]

## Feature links

- [aura-wm/overview-mode](../06-features/aura-wm/overview-mode.md)

## Sources

- S-3f3c8ab94769 · 2026-07-18 · Chromium ash overview source and equivalence inventory
- S-1f554a1713a5 · 2026-07-18 · Lift-map evaluation and corrections
- S-4a6520490f9a · 2026-11-09 · Pinned donor interface inspection
- S-4ea478cdaf86 · 2026-11-09 · Pinned donor interface inspection
- S-e19473fe102a · 2026-11-09 pin check · Pinned overview transform and clip
