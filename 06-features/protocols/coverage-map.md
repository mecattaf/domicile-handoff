---
id: protocols/coverage-map
title: Wayland protocol coverage map
kind: collection
family: protocols
layers: []
domicile_status: unknown
domicile_refs: []
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
scroll_parity: n/a
donors: []
eras: []
rows: []
depends_on: []
evidence_mix: mostly-inferred
boundary_note: false
bucket: domicile
tags: [family:protocols, status:unknown, bucket:domicile]
sources: [S-255b69aec99d, S-26ca51e574f3, S-2b648ac0864e, S-47559b148b0b, S-476d5620b7bd, S-4c46e40f55d9, S-6299b9bca7d0, S-703c0d408b29, S-73c110839629, S-922b92cf0ccf, S-a73d64dc0796, S-aafc0349dee2, S-b8455d5ff8d2, S-c6382b128a37, S-e463b7863c97, S-f325935ceeea]
---
# Wayland protocol coverage map

Rows and findings only.

## What the notes found
- 2026-11-09: The checked Ozone registry handler binds alpha compositing, keyboard control and extended dragging protocols; it contains no zaura binding branch (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_connection.cc#L663> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_connection.cc#L714> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_connection.cc#L763>
- 2026-11-09: Exo separates direct SharedImage resources from copied textures. The copy path releases source contents after its copy query, while direct resources follow the returned-resource export result (exo@engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/buffer.cc#L426> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/buffer.cc#L738> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/buffer.cc#L761>
- 2026-11-09: Exo handles frame acknowledgment separately from frame-token presentation feedback. Acknowledging a submitted frame is therefore a different source event from successful presentation; sink loss sends failure feedback (exo@domicile-engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/layer_tree_frame_sink_holder.cc#L188> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/layer_tree_frame_sink_holder.cc#L212> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/layer_tree_frame_sink_holder.cc#L229>
- 2026-11-09: The inspected Exo Server::Initialize registers the standard clipboard data-device manager but no primary-selection manager. Its Wayland target separately asserts ChromeOS (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/server.cc#L236> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/server.cc#L257> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/BUILD.gn#L12>
- 2026-11-09: Exo caps xdg_wm_base binding at version 3; reactive positioner and parent-size/configure handlers are empty, while explicit popup reposition recalculates bounds and sends repositioned/configure events (exo@domicile-engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/xdg_shell.cc#L100> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/xdg_shell.cc#L532> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/xdg_shell.cc#L880>
- 2026-11-09: Exo registers xdg_wm_base version three and text-input version one in the checked server; these are server declarations, separate from Domicile Smithay support (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/server.cc#L349> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/server.cc#L362>
- 2026-11-09: Exo’s WMHelper obtains focus from Ash and registers pre-target handlers directly on Ash Shell. Its server-side protocol semantics are coupled to that window authority, beyond the ChromeOS build gate (exo@engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wm_helper.cc#L253> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wm_helper.cc#L271>
- 2026-11-09: Exo registers its compositor global using the buffer-scale request version constant, rather than the version-six target asserted by some historical matrices (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/wl_compositor.h#L14> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/server.cc#L239>
- 2026-11-09: The native Wayland client creates a per surface alpha and blending object when the Chromium alpha protocol is bound. Pending opacity and blend state changes emit alpha and premultiplied or disabled blending requests (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_surface.cc#L160> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_surface.cc#L566>
- 2026-11-09: The native window drag controller prefers a bound standard toplevel drag manager. Otherwise it uses the Chromium extended drag protocol, sending the dragged surface and its offset through the vendor drag source (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_window_drag_controller.cc#L216> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_window_drag_controller.cc#L96>
- 2026-11-09: At the pin, shared-memory native windows use GPU-copy or software-memory import paths. Software fallback withdraws dmabuf advertisement; GL clients connected before fallback remain a documented gap [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WINDOW-COMPOSITING.md#L295> <https://github.com/cprussin/domicile/pull/1856>
- 2026-11-09: Exo’s build requires both Ozone and is_chromeos. Its server-side protocol semantics do not establish that the code is compiled in Domicile’s Linux engine (exo@150.0.7871.124) [source] <https://github.com/chromium/chromium/blob/150.0.7871.124/components/exo/BUILD.gn#L12>
- 2026-11-09: Background-effect blur requests copy a clipped surface-local region; an empty or null region removes the effect, and committed state is double buffered (wayland-protocols@1.48) [source] <https://gitlab.freedesktop.org/wayland/wayland-protocols/-/blob/1.48/staging/ext-background-effect/ext-background-effect-v1.xml>
- 2026-11-09: Exo chooses the DMA BUF modifier request version when GPU capabilities lack a DRM device identity, and the feedback request version when the identity is present; binding clamps the client version to this platform limit (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/wayland_dmabuf_feedback_manager.cc#L357>

