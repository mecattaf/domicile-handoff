---
id: clipboard-dnd/data-control-and-primary-selection
title: Clipboard, primary selection and drag-and-drop
kind: feature
family: clipboard-dnd
layers: [engine:browser, compositor, protocol, sdk, manganese]
domicile_status: landed-with-gap
domicile_refs: [cprussin/domicile#1162, cprussin/domicile#525, cprussin/domicile#530, cprussin/domicile#950]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, exo, smithay]
rows: [ext-data-control-v1, primary-selection-v1, wl_data_device_manager, wlr-data-control-v1]
donor_availability: semantics-only
evidence_mix: mostly-source
boundary_note: true
bucket: domicile
tags: [family:clipboard-dnd, status:landed-with-gap, evidence:source, scroll-parity:baseline]
sources: [S-0aefaf2f5972, S-1231eab907ed, S-1601ddb380e7, S-1951a846e891, S-291dce183eb2, S-33889728083c, S-3c3fac3c1939, S-3d63fd27af2d, S-4111d1fe471f, S-489f5a8cafed, S-548e570973fa, S-594f7c37cd88, S-62b183b89469, S-73bd30934d13, S-7506f62591a2, S-7db6c4abc1a7, S-82b445bd2404, S-830c16d0468b, S-85696667a154, S-87597e238c57, S-9a96a8a8b6e2, S-abb7cef0dd30, S-b18ff0a38532, S-b342c7789315, S-bad1e7700ff9, S-bb76c1075d0e, S-c153d333f325, S-c2596d00b100, S-d2f3bb1397d2, S-da9b171d79ff, S-df214f6d3232, S-e0a6e2a8d6f1, S-e52ad911c3ac, S-ef77e5cd4455, S-f4f9c94f2063, S-fc72acc31b12]
---
# Clipboard, primary selection and drag-and-drop

The compositor owns clipboard selections and a memory-only history. Primary selection and drag-and-drop keep their own roles, even though Wayland data-device machinery supplies related offers and transfers. [clipboard contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/docs/CLIPBOARD.md#L14) [docs]

## Behavior

- History retains recent text and image copies and can serve a selected entry after the source application exits. Primary selection is excluded from history. [clipboard contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/docs/CLIPBOARD.md#L22) [docs]
- Both ext and wlr data-control protocols let an unsandboxed clipboard manager access selections without keyboard focus. A restored history entry is also available through data control. [clipboard manager contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/docs/CLIPBOARD.md#L42) [docs]
- On a tty, the engine and Wayland clients share the compositor clipboard. Nested browser content still uses the host clipboard. [clipboard browser boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/docs/CLIPBOARD.md#L53) [docs]

- A native drag starts only with a matching pointer or touch implicit-grab serial. Smithay assigns the drag-icon role, invokes the compositor callback and installs its drag grab. [start-drag source](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/selection/data_device/device.rs#L64) [source]
- The Smithay grab sends target motion and delivers a valid drop or cancels the source. Finishing a transfer validates the negotiated action before notifying the source. [drag lifetime source](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/selection/data_device/dnd_grab.rs#L216), [transfer finish source](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/selection/data_device/dnd_grab.rs#L579) [source]

- Native MIME offers remain available through Smithay independently of the engine/history snapshot. That snapshot selects UTF-8 text and one image, preferring PNG. A cleared or unsupported-format selection leaves its previous retained copy intact. [source] [Native offer relay](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/selection/offer.rs#L138) · [Snapshot formats](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/clipboard.rs#L130) · [Selection snapshot](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/selections.rs#L65)
- History retention is bounded to 32 entries and 128 MiB overall, with 1 MiB text and 32 MiB image limits. An oversized offer can still paste directly from its source while being omitted from history. [source] [Retention limits](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/clipboard.rs#L13)

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Drag-icon scene nodes live beneath a per-seat subtree | Pointer icons follow the cursor; touch icons require the drag contact and use stored seat touch coordinates | [Icon position](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/seat.c#L362) · [Scene node](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/seat.c#L456) · [Touch update](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/seat.c#L519) |
| scroll | Retains the per-seat drag-image scene and contact check | Drag-image rendering is a separate compositor responsibility from offer transfer | [Icon position](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/seat.c#L364) · [Scene node](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/seat.c#L458) · [Touch update](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/seat.c#L521) |

## What the notes found

- A clipboard manager access path differs from focused copy/paste. The protocol map records primary selection separately from clipboard and data-control managers. [docs]
- Exo sends drag initiation and selection changes to separate seat operations; focused-surface state controls clipboard offers. Its data-device object combines drag/drop and clipboard observation. This is a semantics-only donor: Exo is ChromeOS-gated and Smithay serves Domicile clients. [pinned Exo source](https://github.com/chromium/chromium/blob/150.0.7871.124/components/exo/data_device.cc#L78) [source]
- 2026-11-09: Exo DataDevice handles clipboard selection separately from drag entry, update and drop callbacks; portability still inherits the Exo build gate (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/data_device.cc#L80> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/data_device.cc#L87> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/data_device.cc#L166> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/BUILD.gn#L13>
- 2026-11-09: Clipboard selection-buffer support is false on ChromeOS, while non-ChromeOS Ozone builds query the active clipboard backend. Exo Seat::SetSelection writes the separate kCopyPaste buffer (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/base/clipboard/clipboard.cc#L88> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/seat.cc#L222> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/seat.cc#L271>
- 2026-11-09: The native clipboard ABI copies MIME-keyed payloads before posting across its engine thread. Copied-item callbacks borrow data only for dispatch; a legacy callback receives text from the designated text MIME entry (Domicile native engine at fixed pin) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L962> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L1294> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L2096>
- 2026-11-09: DataExchangeDelegate classifies transfer endpoints, selects URI-list MIME types, detects serialized filesystem URLs and parses file sources. These four methods do not define a primary-selection ownership store; Exo remains ChromeOS-gated (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/data_exchange_delegate.h#L27> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/BUILD.gn#L13>
- 2026-11-09: Ash clipboard history preserves an existing item’s identity when equivalent content moves to the front; entries beyond its five-item capacity are evicted from the back (ash@domicile-engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/clipboard/clipboard_history.cc#L216> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/clipboard/clipboard_history_util.h#L28>
- 2026-11-09: Ash debounces multiple clipboard writes within one task sequence before committing the latest data; nested pauses follow the newest active pause’s behavior (ash@domicile-engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/clipboard/clipboard_history.cc#L83> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/clipboard/clipboard_history.h#L95>
- 2026-11-09: Ash clipboard-history removal synchronizes the system clipboard to the remaining newest item; clearing history can separately choose whether to reset the system clipboard (ash@domicile-engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/clipboard/clipboard_history.cc#L51> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/clipboard/clipboard_history.cc#L190>
- 2026-11-09: The browser broker stores no clipboard snapshot for new clipboard observers; they receive later copy notifications. Display observers instead receive a stored nonempty display list on registration (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/frame_sink_broker.cc#L314> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/frame_sink_broker.h#L183>
- 2026-11-05: The pinned compositor implements wlr and ext data-control, and wl-clipboard works against it; image transfer from Chrome through the clipboard was untested [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/selections.rs>
- 2026-11-05: sway's primary_selection toggle is enabled by default and can only be configured at launch. [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L863-L865>
- 2026-10-06: GTK distinguishes same-client typed drop payloads from cross-client transfer; libadwaita moves typed tab-page objects between windows of one process (gtk@84abc9b1;libadwaita@19098711) [docs] <https://github.com/GNOME/gtk/blob/84abc9b1/gdk/wayland/gdkdrop-wayland.c#L98> <https://github.com/GNOME/libadwaita/blob/19098711/src/adw-tab-box.c#L2599>
- 2026-10-23: Terminal-backend clipboard writes use OSC52 and paste arrives as bracketed terminal paste; clipboard reads return the application copy cache. Those semantics differ from querying a desktop clipboard owner (a native shell spike's terminal backend, 2026-10-23) [docs]
- 2026-06-12: June coverage records cross-surface drag and drop as a design gap; a clipboard data donor alone does not establish the complete drag lifecycle (Chromium historical studies) [docs]
- 2026-06-29: The June primary-selection draft checks generated protocol symbols against an Ozone test server, then sketches a new Exo registration. Vendored XML and test symbols do not establish production server support; the draft also records initial-focus and offer-source lifetime issues (Chromium historical studies) [docs]
- 2026-10-02: The October Views document distinguishes a notification target’s direct D-Bus dependency from full program closure: the clipboard path separately reaches the XDG FileTransfer portal’s bus connection (views-shell@October-2026) [docs]

## Where it touches Domicile

- `compositor` keeps complete supported text/image snapshots and sends the shell only identifiers and bounded previews. [clipboard storage contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/docs/CLIPBOARD.md#L22) [docs]
- `engine:browser` on a tty receives pushed selections through the DRM clipboard bridge. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/A-DESKTOP-ON-A-TTY.md#L159) [docs]
- Status at the pin: landed-with-gap. ext/wlr data control and primary selection are served; the nested engine clipboard boundary is recorded in the known gaps of [#530](https://github.com/cprussin/domicile/pull/530). [nested clipboard limit](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/docs/CLIPBOARD.md#L53) [docs]

- Drag-and-drop scope: the pinned Domicile client/server drag callbacks use empty implementations. Smithay default callbacks perform no compositor-specific drag-image work. The source establishes protocol transfer machinery; complete drag-image rendering and browser/shell interaction remain unknown here. [Domicile callbacks](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/selections.rs#L130), [Smithay defaults](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/selection/data_device/mod.rs#L126) [source]

- At the browser broker seam, a new clipboard observer receives later copy notifications without a saved snapshot being replayed. The native ABI copies MIME-keyed payloads before posting across the engine thread; callback buffers are borrowed for dispatch. This observer lifetime is separate from the compositor history above. [source] [Observer registration](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/frame_sink_broker.cc#L314) · [Broker state](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/frame_sink_broker.h#L183) · [Payload copy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L962) · [Callback lifetime](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L1294)

## Lineage

- 2026-10-06: [Unfocused clipboard-manager access](https://github.com/cprussin/domicile/pull/950) merges upstream. [docs]

- 2026-10-11: [Text and image clipboard snapshots](https://github.com/cprussin/domicile/pull/1162) merge upstream. [docs]

- 2026-09-25: [Browser and compositor clipboard bridge](https://github.com/cprussin/domicile/pull/530) merges upstream. [docs]

- 2026-09-22: [Primary selection](https://github.com/cprussin/domicile/pull/525) merges upstream. [docs]

The protocol history is indexed by collection.

Typical clients: screenshot and recording tools, remote desktop, clipboard managers, accessibility and test automation.

## Sources

- S-c153d333f325 · 2026-11-09 · Native engine clipboard and capture transfer lifetimes.

- S-0aefaf2f5972 · 2026-11-09 · Browser broker retained-state fields.

- S-ef77e5cd4455 · 2026-11-09 · Browser clipboard observer registration and display replay.

- S-82b445bd2404 · 2026-11-09 · Smithay native MIME offer relay.

- S-85696667a154 · 2026-11-09 · Scroll per-seat drag-image scene and touch position.

- S-df214f6d3232 · 2026-11-09 · Sway per-seat drag-image scene and touch position.

- S-e0a6e2a8d6f1 · 2026-10-06 · Unfocused clipboard-manager access merge record.

- S-b342c7789315 · 2026-10-11 · Text and image clipboard snapshots merge record.

- S-fc72acc31b12 · 2026-09-25 · Browser and compositor clipboard bridge merge record.

- S-548e570973fa · 2026-09-22 · Primary selection merge record.

- S-87597e238c57 · 2026-11-09 · Supported clipboard snapshots and retention limits.

- S-594f7c37cd88 · 2026-11-09 · Native MIME offer relay.

- S-1231eab907ed · 2026-11-09 · Exo clipboard and drag operation dispatch at Chromium 150.
- S-1601ddb380e7 · 2026-11-09 · Smithay start-drag implicit-grab validation.
- S-1951a846e891 · 2026-11-09 · Direct-session architecture and native device ownership.
- S-291dce183eb2 · 2026-11-09 · Smithay data-device manager and default drag callbacks.
- S-3d63fd27af2d · 2026-11-09 · Clipboard storage, selection and browser boundary contract.
- S-62b183b89469 · 2026-11-09 · Exo data-device drag and clipboard interfaces at Chromium 150.
- S-abb7cef0dd30 · 2026-11-09 · Smithay drag target, drop and transfer-finish lifetime.
- S-bad1e7700ff9 · 2026-11-09 · Clipboard history user contract.
- S-c2596d00b100 · 2026-11-09 · Portal interfaces, direct capture and sandbox boundary.
- S-e52ad911c3ac · 2026-11-09 · Domicile drag callbacks and selection serving.
