---
id: browser-windows/tab-groups-and-strips
title: Window groups and strip behavior
kind: feature
family: browser-windows
layers: [engine:browser, sdk, manganese]
domicile_status: unknown
checked_at: "2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc"
answered_upstream: false
scroll_parity: n/a
donor_availability: rule-portable
donors: [views]
evidence_mix: mixed
boundary_note: false
bucket: domicile
tags: [family:browser-windows, donor:views, evidence:source, bucket:domicile]
sources: [S-00f961d63411, S-0aa579827d73, S-1f02c51a3137, S-20018859cea2, S-3abebbad32a3, S-489ead0591d3, S-4abf73f29481, S-62b1b37d6a91, S-695f43c6be84, S-772087eeff37, S-82d30cf5a21b, S-8a16f18cd20c, S-9545c02d449b, S-bcc52862f645, S-c4fb3983e0e6, S-cc179bf996c8, S-d2175a3f9781, S-d31169455550, S-d5b15decb594, S-f9fef5948cb2]
eras: [views-shell]
rows: [AL-views-shell-vtabs-011, AL-views-shell-vtabs-012, AL-views-shell-vtabs-013, AL-views-shell-vtabs-014, AL-views-shell-vtabs-015, AL-views-shell-vtabs-016, AL-views-shell-vtabs-017, AL-views-shell-vtabs-018, AL-views-shell-vtabs-019, AL-views-shell-vtabs-020, AL-views-shell-vtabs-021, AL-views-inventory-041, AL-views-inventory-045, AL-views-inventory-055, AL-views-inventory-056, AL-views-inventory-062, AL-engine-check-007]
depends_on: []
reported_on_domicile: []
domicile_refs: [docs/SHELL-BROWSER-WINDOWS.md, docs/architecture/STACKING-PARITY.md, packages/manganese-window-management/docs/KEYS.md]
---
# Window groups and strip behavior

A group preserves window membership while its visible strip changes state. Expanding, collapsing and destroying a strip can affect focus and popup ownership without changing which application window belongs to the group. [inferred]

## Behavior

- Chrome’s collection-removal path disables subtree events and focus before exit animation; pending views remain until the starting layout no longer references them, then removal refreshes hover. [source] ([Donor implementation 1](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/tab_collection_animating_layout_manager.cc#L634); [Donor implementation 2](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/tab_collection_animating_layout_manager.cc#L1036)).

- Temporary expansion state has a lifetime distinct from group membership. [source] ([Lock type and lifetime](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/vertical/vertical_tab_strip_expand_on_hover_lock.h#L16))
- Structural model changes have explicit drag cancellation behavior. [source] ([Cancellation contract](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/tab_drag_handler.h#L110))
- Exit drawing can outlive logical removal: child destruction is deferred while animated effects still use it. [source] ([Removal lifetime](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/tab_collection_animating_layout_manager.h#L143))

- [First/last tab moves](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/tab_strip_collection_controller.cc#L205) respect pinned ranges and focused-group bounds. Leaving a focused group can also change another group membership while the tab index stays fixed. [source]
- [Collapsing the active group](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/tab_strip_collection_controller.cc#L371) selects an expanded alternative or requests a new tab outside it; a weak reference guards synchronous group destruction. [source]
- A tab context menu [holds the current expansion state](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/tab_strip_collection_controller.cc#L462) and [releases its lock on closing](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/tab_strip_collection_controller.cc#L585). [source]

## How donors do it

| Donor @ pin | Mechanism | Notable behavior or limit | Link |
|---|---|---|---|
| Chromium Views @ engine pin | An expand-on-hover lock has an explicit lifetime and lock type | Parent destruction clears the referenced strip view before the lock can act on it | [Expansion lock](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/vertical/vertical_tab_strip_expand_on_hover_lock.h#L16) |
| Chrome tab strip @ engine pin | TabDragHandler integrates drag state with the browser model | Adding a tab or removing a dragged tab cancels the drag; BrowserRootView, TabStripModel and WebContents remain dependencies | [Model changes](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/tab_drag_handler.h#L110) |

The current collection controller [holds BrowserView and TabStripModel references](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/tab_strip_collection_controller.cc#L67) and reaches profile services. Root collection nodes [observe browser collections](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/views/tabs/common/root_tab_collection_node.cc#L40) and pass WebContents into drag handling. These implementations assume the browser model beyond the generic Views toolkit. [source]

## What the notes found

- The 2026-10-01 catalog identified vertical-tab behaviors separately from the component palette. Its client-shell crosswalk replaced Aura window ownership with a compositor-fed model. [docs] S-20018859cea2 · S-82d30cf5a21b.
- 2026-11-08: The Manganese tab strip and per-window title bar are React DOM elements styled with Panda CSS, not Chrome's C++ tab strip and not the generic Tabs component of the component library [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/TabStrip.tsx> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/TitleBar.tsx>
- 2026-11-09: Manganese window rectangles include a 30-pixel title bar; the native surface extends one pixel under it, so content allocation subtracts 29 pixels from the enclosing height [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/rect.ts#L14>
- 2026-11-08: Manganese places every tiled window in a tab group, including a lone window; a split holds only groups, and a new window opens as a tab after the focused one. The same rule was documented at the 2026-11-03 baseline [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L14>
- 2026-11-09: Manganese’s tiled-window model retains a tab group for a single window (Domicile@pin) [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L164>
- 2026-11-08: Each window keeps its own title element while the shown window renders the shared rail behind the tabs, so title elements persist as windows move between groups [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/TabStrip.tsx>
- 2026-11-08: Hiding the visible strip with styling alone leaves the group-per-window tree underneath, so a layout without tab groups touches window policy as well as presentation [inferred] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/tree/grouped.ts>
- 2026-10-06: A Views-on-Otto study retained tab sizing arithmetic while rejecting runtime linking across different Skia milestones, C++ runtimes and Wayland connections (otto@v1.6.0) [docs]
- 2026-10-06: The tab-policy study separated width arithmetic from container-owned input and drag state; a compositor closing a native client also requires a pending-close lifetime absent from in-process browser removal (chromium@154.0.8037.92) [docs]
- 2026-11-04: Domicile ships one Panda preset, shared components with Base UI behavior and a TabRail component that serves as the React base for tab strips; workspace capabilities and confirmation sit in adapters [docs]

## Where it touches Domicile

- The [browser-window contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-BROWSER-WINDOWS.md) and [stacking contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/STACKING-PARITY.md) define the current Domicile grouping boundary.
- State, model ownership and temporary presentation have separate lifetimes. The browser-bound drag controller supplies semantics. [inferred]

- Manganese retains a tab group even for a single tiled window. This current group model is separate from the browser-bound Chrome strip controllers. [docs] [Current grouping contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L164).

## Lineage

- 2026-10-01: the Views catalog connected strip behavior to workspace membership while keeping authority in the compositor model. [docs]

## Sources

- S-772087eeff37 · 2026-11-09 · Chromium strip expansion lifetime interface
- S-f9fef5948cb2 · 2026-11-09 · Chromium collection drag and model-change interface
- S-1f02c51a3137 · 2026-11-09 · Chromium collection animation and deferred-destruction interface
- S-20018859cea2 · 2026-10-01 · Views component and behavior catalog
- S-82d30cf5a21b · 2026-10-01 · Ash and client-shell window authority study
- S-d5b15decb594 · 2026-11-09 · Domicile tiled-group and keyboard model
- S-4abf73f29481 · 2026-11-09 · Chromium collection exit drawing and input lifetime
- S-489ead0591d3 · 2026-11-09 · Chromium tab-model coupling, movement and collapse lifetimes
- S-62b1b37d6a91 · 2026-11-09 · Chromium root tab-collection observers and drag ownership
- S-0aa579827d73 · 2026-11-09 pin check · Pinned browser-window shell contract
- S-3abebbad32a3 · 2026-11-09 pin check · Pinned stacking and transient-window contract
