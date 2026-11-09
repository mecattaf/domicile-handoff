---
id: windows/content-scaling
title: Per-window content scale
kind: feature
family: windows
layers: [engine:browser, engine:blink, manganese, compositor]
domicile_status: absent
domicile_refs: [cprussin/domicile#1944, packages/manganese-window-management/docs/KEYS.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [scroll]
eras: []
rows: [SC-0062, SC-0078, SC-0290, SC-0291, SC-0292, SC-0293, SC-0521, SC-0524, SC-0525, SC-0526, SC-0614, SC-0615, SC-0706]
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: false
bucket: domicile
tags: [family:windows, layer:engine:browser, status:absent, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-083adea7e7ad, S-1fa1acbff487, S-204a969601d0, S-34fe9c520452, S-3533e3f0274a, S-39a38ba5f505, S-3e3acf139049, S-3f62cd65b606, S-5b1bd336dbff, S-7906dff47c93, S-977679cef075, S-a70560fdc087, S-ae44137206ee, S-c153d333f325, S-d5b15decb594, S-e4ff18df1f16, S-e75ff934f342, S-f3f3495bad04, S-fd0c87388c02]
---
# Per-window content scale

Content scaling changes a window’s content density independently of monitor scale and overview transforms.

## Behavior

- Exact, incremental and reset operations act on one window.
- Client buffer density and the apparent size of content are tracked separately from the containing output.
- Browser page zoom reflows content; a workspace overview transform changes presentation without becoming content zoom.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| scroll @ pinned source | scale_content and preferred client density | Wayland content scale affects preferred buffer density; the Xwayland path clamps values below one. | [Content scale](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L827) · [Density negotiation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/transaction.c#L2219) |

## What the notes found

- Checked 2026-11-09: Scroll scales individual native windows as well as browser applications; Domicile’s browser pinch slice updates browser zoom in a `<webview>` and leaves native-window pinch behavior unchanged. [source] [Scroll scale](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L827) · [#1944](https://github.com/cprussin/domicile/pull/1944)
- Checked 2026-11-09: The SDK bounds operation forwards the element’s CSS box; the native-window record declares no independent content-density factor. [source] [Bounds operation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/windows.ts#L91) · [Native record](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1139)
- Checked 2026-11-09: Browser pinch updates Chromium browser zoom through HostZoomMap. The setter changes an existing temporary frame override when present, otherwise host zoom; neither path supplies a general native-window density factor. [source] [Browser zoom scope](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/web_view_guest.cc#L1554) · [Browser zoom scope](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/content/browser/host_zoom_map_impl.cc#L452)
- Checked 2026-11-09: Native app layout pixel-snaps the element’s content size to compositor-layer bounds. Page geometry determines the element’s dimensions independently of the previous client buffer. [source] [Native element geometry](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/layout_app_surface.cc#L15)
- Checked 2026-11-09: Domicile chooses client density from the display with the largest overlapping area. Presentation feedback separately uses the first entered output in output-list order. [source] [Density and feedback selection](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/frame_pacing.rs#L134) · [Density and feedback selection](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screens.rs#L752) · [Density and feedback selection](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/screens.rs#L777)
- Checked 2026-11-09: Client viewport state commits with the buffer: a source crop supplies cropped dimensions, while an explicit destination supplies the logical surface size. This client protocol state is separate from a shell content-density command. [source] [Committed viewport state](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/viewport.rs#L27) · [Committed viewport state](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/viewport.rs#L43) · [Committed viewport state](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/surface_commit.rs#L244)
- Checked 2026-11-09: Scroll’s preferred fractional buffer density multiplies the maximum current-output scale by the per-view content factor, with a floor of one. Integer buffer scale rounds that density upward. [source] [Scene density](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/types/scene/surface.c#L25)
- 2026-11-09: Scroll computes preferred fractional density as max(1, max(output.scale * view_content_scale)) and integer buffer scale as ceil(density). Frame pacing separately selects the highest-refresh nonsuspended current output for presentation feedback and frame callbacks (scroll) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/types/scene/surface.c#L25> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/types/scene/surface.c#L156> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/types/scene/surface.c#L39> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/subprojects/wlroots/types/scene/surface.c#L182>
- 2026-11-09: Domicile normalizes client buffer orientation inside native frame construction, including quarter-turn dimension swaps. This source path describes buffer orientation and does not establish page CSS transforms or per-window content scaling (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/surface_transform.h#L27> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/surface_transform.cc#L54> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/engine/domicile_engine.cc#L302>
- 2026-11-09: Domicile applies viewport state with the committed buffer. A source-only viewport sets the cropped surface dimensions; an explicit destination supplies the logical surface size [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/viewport.rs#L27> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/viewport.rs#L43> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/surface_commit.rs#L244>
- 2026-11-09: PR 1944 is merged and describes pinch zoom of the browser window beneath the gesture. Its merge precedes the pinned main snapshot [docs] <https://github.com/cprussin/domicile/pull/1944>
- 2026-11-06: Scroll's content scaling changes the native client's rendering density; Domicile had scene transforms and inverse pointer mapping, but a native density contract for exact parity was not identified (scroll) [docs]

## Where it touches Domicile

- `engine:browser`: a pinch over a browser guest updates that guest’s browser zoom, clamped to Chromium’s browser range. [source] [#1944](https://github.com/cprussin/domicile/pull/1944)
- `engine:blink`: the root desk page does not page-scale from a touchpad pinch. [source] [#1944](https://github.com/cprussin/domicile/pull/1944)
- `compositor`: native-window display density follows output overlap; this is a different contract from a per-window content-scale command. [source] [Client density](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md#L124)
- Status: `absent` for Scroll’s general native-window content-scale command. The absence search is the pin’s full Manganese command inventory; browser-only zoom is a landed subset. [source] [Commands](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L38) · [#1944](https://github.com/cprussin/domicile/pull/1944)

## Lineage

- 2026-11-08: browser pinch handling moved from the desktop to the targeted browser guest. [source] [#1944](https://github.com/cprussin/domicile/pull/1944)

## Sources

- S-204a969601d0 · 2026-11-06 · sway and Scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · [Window command vocabulary and default bindings](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L1).
- S-e4ff18df1f16 · 2026-11-09 · [Desk-wide page and display coordinate ownership](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ONE-PAGE-FOR-THE-DESK.md#L1).
- S-fd0c87388c02 · 2026-11-08 · Public Domicile change catalog.
- S-1fa1acbff487 · 2026-11-09 · [Native window identity, bounds and focus namespace](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/windows.ts#L1).
- S-a70560fdc087 · 2026-11-09 · [Typed native-window and desktop metadata](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1).
- S-3e3acf139049 · 2026-11-09 · [Browser guest zoom routing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/web_view_guest.cc#L1).
- S-977679cef075 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-5b1bd336dbff · 2026-11-09 · [Native element layout and embedded-surface sizing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/layout_app_surface.cc#L1).
- S-083adea7e7ad · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-f3f3495bad04 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-39a38ba5f505 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-e75ff934f342 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-3f62cd65b606 · 2026-11-09 · pinned desktop contract and donor source comparison.
