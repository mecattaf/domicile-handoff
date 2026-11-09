---
id: "02-aura-ash-exo/views-shell-vtabs"
title: "Views shell and strip behavior"
kind: "collection"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile", "evidence:source"]
sources: [S-11b2a3b72dc5, S-1f554a1713a5, S-232e7580bd4e, S-8b26d131e137, S-bf0d8d0612c6, S-c204c4a39cf5, S-f9fef5948cb2, S-0aa579827d73]
---

# Views shell and strip behavior

Views and Chrome strip controllers preserve interaction and lifetime rules. The historical vertical-tab source paths changed at the engine pin. [docs]

## What Chromium already has

The [row data](rows/aura-lift.tsv) holds 19 rows for this beat: ADAPT 8, LIFT 11. These are historical source verdicts. [docs]

Donor availability in those rows: `in-engine-build` 8, `rule-portable` 11. Zero Ash references in a file do not bypass its target build gate. [docs]

- NativeWidgetAura hosts a non-desktop widget without owning a native platform window; ordinary non-ChromeOS usage constrains it to a desktop widget ancestor. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/views/widget/native_widget_aura.h#L49).
- The tab collection node tree now lives under the common tab directory and still has tab, split, group and container node types with injectable view-ownership callbacks. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/tab_collection_node.h#L30).
- The root tab node observes both collection changes and the browser tab model; adapting the concept does not remove those browser-model dependencies automatically. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/root_tab_collection_node.h#L23), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/root_tab_collection_node.h#L60).
- TabStripCollectionController and TabDragHandler provide current controller and drag seams; their signatures retain BrowserView, TabStripModel and WebContents dependencies. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/tab_strip_collection_controller.h#L46), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/tab_drag_handler.h#L44).

## Domicile at the pin

- `engine:browser and manganese`: The browser retains guest pages across shell reloads. Grouping and strip interaction are shell policy over those engine-owned resources. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-BROWSER-WINDOWS.md#L15).

## Boundary

Hover expansion locks, membership, focus and close behavior transfer as concepts. [inferred]

## Feature links

- [browser-windows/tab-groups-and-strips](../06-features/browser-windows/tab-groups-and-strips.md)

## Sources

- S-8b26d131e137 · 2026-07-18 · Chromium views shell and strip behavior source and equivalence inventory
- S-1f554a1713a5 · 2026-07-18 · Lift-map evaluation and corrections
- S-11b2a3b72dc5 · 2026-11-09 · Pinned donor interface inspection
- S-232e7580bd4e · 2026-11-09 · Pinned donor interface inspection
- S-bf0d8d0612c6 · 2026-11-09 · Pinned donor interface inspection
- S-c204c4a39cf5 · 2026-11-09 · Pinned donor interface inspection
- S-f9fef5948cb2 · 2026-11-09 · Pinned donor interface inspection
- S-0aa579827d73 · 2026-11-09 pin check · Pinned browser-window shell contract
