---
id: capture/screencopy-and-image-capture
title: Screen and window capture
kind: feature
family: capture
layers: [engine:viz, compositor, protocol, sdk, any-shell, external]
domicile_status: landed
domicile_refs: [cprussin/domicile#1107, cprussin/domicile#1510]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, smithay]
rows: [ext-foreign-toplevel-image-capture-source-v1, ext-image-capture-source-v1, ext-image-copy-capture-v1, ext-output-image-capture-source-v1, wlr-export-dmabuf-v1, wlr-screencopy-v1]
evidence_mix: mostly-source
boundary_note: true
bucket: domicile
tags: [family:capture, status:landed, evidence:source, scroll-parity:baseline]
sources: [S-10cfd46e0cfd, S-24f31085af99, S-2d260845be4d, S-43af2b3af183, S-5cf90666708b, S-5d60bfe102cf, S-5eabf16d1832, S-5fb9706b471e, S-83c6a5bbb592, S-8cc20f695fcd, S-8f488b01f5b6, S-bfec45af03af, S-c153d333f325, S-c2596d00b100, S-c34c6ce697a3, S-c5d3db1884f1, S-cb7bd5cf8a30, S-ceda35169c1d, S-de6569bcc761, S-e7392f226f69, S-eecd68f7af94, S-ef6c7832a511, S-ef77e5cd4455]
---
# Screen and window capture

Screen and window capture share the compositor casting machinery through two interfaces: permission-mediated portals and direct Wayland capture protocols. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L499) [docs]

## Behavior

- The portal supplies monitors, regions, native windows and browser windows as PipeWire streams. Browser page capture can remain active while that window is hidden. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L321) [docs]
- Direct Wayland capture advertises wlr screencopy version 3 and ext image-copy capture with output and foreign-toplevel sources. Its toplevel list includes native Wayland windows, excluding browser windows. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L501) [docs]
- Capture supports shared-memory and dmabuf frames, damage and embedded pointer images. A resize replaces buffer constraints; unplugging a source stops its sessions. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L506) [docs]
- Unsandboxed clients can capture without a dialog. Sandboxed clients do not see direct capture globals and use portals instead. Capture may continue while locked. The upstream contract describes lock-screen imagery; the pixels produced by each isolated native-window source while locked remain unverified. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L527) [docs]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Scene-backed screencopy and image-copy sources | Output and toplevel capture are separate source types | [pinned server](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/server.c#L511) |
| scroll | Retains sway capture protocols | Output and toplevel sources share the scene-backed path | [pinned server](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/server.c#L514) |

## What the notes found

- The map keeps direct capture and portal permission as different contracts. [inferred]
- 2026-11-09: Native capture frames retain a broker hold until compositor release. The payload forwards DMA-BUF plane descriptors or a shared-memory descriptor, together with format, content rectangle and damage rectangle (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L521> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L562> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L566> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L603>
- 2026-11-09: Native display capture rejects zero size or rate before requesting the broker; browser-window capture rejects a null identifier or zero rate. Starts wait for the broker answer; resizing and frame release are posted to the engine thread (Domicile native engine at fixed pin) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L1243> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L1276> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L1349> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L2130>
- 2026-11-09: Wayland capture ready timestamps use CLOCK_MONOTONIC after optional fence completion and staging-buffer copying. They stamp the compositor-ready delivery path; the Filled payload carries no engine presentation timestamp [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/mod.rs#L308> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/mod.rs#L351> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/mod.rs#L483> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/casting/streams.rs#L1647>
- 2026-11-09: Native capture events share ownership of plane descriptors across queued event copies; the last copy closes them. The C callback borrows these descriptors for its callback duration (Domicile native engine at fixed pin) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/engine_event_queue.h#L43> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L986> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L1319>
- 2026-11-09: The native capture contract requires releasing frames after reading. Viz has a limited buffer pool; retaining every frame stalls capture, while stop releases held frames. Descriptor duplication preserves access only until frame release (Domicile native engine at fixed pin) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.h#L139> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.h#L358> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.h#L752>
- 2026-11-09: Native-window ARGB8888/XRGB8888 shared-memory buffers have a CPU path. Other shared-memory formats and native dmabufs require the optional GLES renderer or return NoGpu. Monitor capture follows a separate buffer path [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/casting/gpu.rs#L81> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/casting/streams.rs#L831> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/casting/captured.rs#L34>
- 2026-11-09: CaptureModeController exposes capture-session start and stop operations; this header does not establish a portable Wayland capture-protocol server (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/capture_mode/capture_mode_controller.h#L240> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/capture_mode/capture_mode_controller.h#L257>
- 2026-11-09: The native capture bridge drops DMA-BUF payloads with a non-native-pixmap handle or more planes than its ABI can carry. Closing the unretained hold returns a rejected frame to the browser (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L574> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L583>
- 2026-11-09: Viz replaces secure-output-only textures with black when the output is insecure or the texture lies in a copy-request pass. Required-overlay content can separately be duplicated into an intermediate capture pass and its parent (Chromium@engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/viz/service/display/surface_aggregator.cc#L1044> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/viz/service/display/surface_aggregator.cc#L1483>
- 2026-11-09: Browser-window capture retains its returned drawing hold until that capture ends. An unavailable browser-window target returns false; the broker path alone does not establish portal permission or wire-protocol behavior (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/frame_sink_broker.cc#L389> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/frame_sink_broker.cc#L416>
- 2026-11-09: Generic Layer output-copy requests without a result task runner require compositor attachment and inherit its task runner. Forwarding a request to the compositor layer does not establish a capture protocol, portal policy or zero-copy behavior (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.cc#L1272> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.h#L481>
- 2026-11-09: Scroll exports client content geometry relative to its native surface separately from container-relative window geometry, allowing captured surface consumers to distinguish client-drawn decoration from content (scroll@b6586d273fed7d187744b7eaec2c234386a029fa) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/ipc-json.c#L623> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-ipc.7.scd#L367>
- 2026-11-09: A valid SubtreeCaptureId identifies a non-root layer subtree for FrameSinkVideoCapturer and forces a separate CompositorRenderPass; capture identity can therefore change render-pass structure (aura@engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/compositor/layer.h#L205>
- 2026-11-09: Browser capture mode is a ChromeOS-gated static library with dependencies on Ash, profiles, enterprise policy, cloud storage, image search and recording services (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chrome/browser/ui/ash/capture_mode/BUILD.gn#L5>
- 2026-11-09: The recording service target also requires ChromeOS; its presence in the source tree does not make it part of a generic Linux engine build (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/chromeos/ash/services/recording/BUILD.gn#L5>

## Where it touches Domicile

- `engine:viz` supplies frames; `compositor` shares casts between PipeWire and direct client buffers. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L506) [docs]
- `protocol` advertises wlr screencopy v3 and ext image-copy/source managers v1. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screencopy/wlr.rs#L35) [source]
- Status at the pin: landed. Browser-window capture exists through the portal; direct foreign-toplevel capture has the stated native-window scope. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L501) [docs]

- [#1107](https://github.com/cprussin/domicile/pull/1107) landed direct Wayland capture, and [#1510](https://github.com/cprussin/domicile/pull/1510) added native-window sources and pointer sessions. The current contract above incorporates subsequent scheduling changes. [docs]

- The native capture bridge retains each frame’s broker hold until compositor release. Payloads carry DMA-BUF planes or shared memory plus format, content and damage rectangles. Unsupported DMA-BUF handle types and excess planes are rejected, releasing that hold. [source] [Frame ownership](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L521) · [Frame fields](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L562) · [Handle validation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L574) · [Plane validation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L583)
- Browser-window capture retains its drawing hold until capture ends. Starting requires an available browser-window target; that broker result alone does not establish portal permission. [source] [Drawing hold](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/frame_sink_broker.cc#L389) · [Target check](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/frame_sink_broker.cc#L416)

## Lineage

- The dated protocol studies record capture interfaces independently of their clients. Typical clients: screenshot and recording tools, remote desktop, clipboard managers, accessibility and test automation. [docs]

## Sources

- S-ef77e5cd4455 · 2026-11-09 · Browser-window capture target and drawing hold.

- S-c153d333f325 · 2026-11-09 · Native capture frame transfer, validation and release.

- S-c5d3db1884f1 · 2026-10-24 · Window-source and pointer-capture merge record.

- S-c34c6ce697a3 · 2026-10-10 · Direct Wayland capture merge record.

- S-43af2b3af183 · 2026-11-09 · Pinned donor implementation locator.

- S-e7392f226f69 · 2026-11-09 · Pinned donor implementation locator.

- S-5fb9706b471e · 2026-11-09 · Wlr screencopy version registration.
- S-c2596d00b100 · 2026-11-09 · Portal interfaces, direct capture and sandbox boundary.
- S-ef6c7832a511 · 2026-11-09 · Image-copy and capture-source manager registration.
