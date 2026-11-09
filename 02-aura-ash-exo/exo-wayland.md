---
id: "02-aura-ash-exo/exo-wayland"
title: "Exo Wayland server and Ozone client"
kind: "collection"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile", "evidence:source"]
sources: [S-063bb48a06e4, S-1f554a1713a5, S-2bb5f68bf0ba, S-2e38f34f8b7b, S-4111d1fe471f, S-4c46e40f55d9, S-662084865ea5, S-a73d64dc0796, S-d9193550e1ab, S-f325935ceeea, S-c2596d00b100, S-76ba87eac499]
---

# Exo Wayland server and Ozone client

Exo registers server globals; Ozone Wayland binds client globals. Protocol names alone do not imply that the two sides implement the same version or members. [docs]

## What Chromium already has

The [row data](rows/aura-lift.tsv) holds 55 rows for this beat: ADAPT 9, FRESH 13, LIFT 33. These are historical source verdicts. [docs]

Donor availability in those rows: `chromeos-gated` 55. Zero Ash references in a file do not bypass its target build gate. [docs]

- SurfaceAugmenter forwards rounded-corner and clip-rectangle state to Exo Surface; the Exo target remains ChromeOS-gated. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/surface_augmenter.cc#L68), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/surface_augmenter.cc#L88).
- Exo still leaves its pointer swipe request unimplemented at this engine pin; its pinch delegate sends real begin, update and end events. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/zwp_pointer_gestures.cc#L30), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/zwp_pointer_gestures.cc#L52).
- The Ozone Wayland wrapper registers pinch and hold listeners; no swipe object or listener appears in the checked wrapper files. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_zwp_pointer_gestures.cc#L72), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_zwp_pointer_gestures.cc#L87).
- The checked Ozone registry handler binds alpha compositing, keyboard control and extended dragging protocols; it contains no zaura binding branch. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_connection.cc#L663), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_connection.cc#L714).

## Native Chromium client paths

These client-side requests belong to Ozone Wayland, separately from the gated Exo server. Actual use depends on the advertised global, its version and the applicable client path. [inferred]

- The native Wayland client creates a per surface alpha and blending object when the Chromium alpha protocol is bound. Pending opacity and blend state changes emit alpha and premultiplied or disabled blending requests. [source] [Client source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_surface.cc#L160), [Client source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_surface.cc#L566).
- The native Wayland client creates an extended keyboard when the Chromium keyboard protocol is bound. It handles peek key events and acknowledges unhandled keys with their serial; peek support also changes duplicate release dispatch. [source] [Client source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_keyboard.cc#L166), [Client source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_keyboard.cc#L457).
- The native window drag controller prefers a bound standard toplevel drag manager. Otherwise it uses the Chromium extended drag protocol, sending the dragged surface and its offset through the vendor drag source. [source] [Client source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_window_drag_controller.cc#L216), [Client source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_window_drag_controller.cc#L96).

## Domicile at the pin

- `compositor`: Domicile documents its own capture globals and security filtering; current protocol status comes from its compositor, not an Exo support cell. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L499).
- `external`: At the engine pin, the Ozone connection binds selected zcr protocols; no zaura binding is present in the inspected connection interface and implementation. [docs] [Pinned contract](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_connection.cc#L663).

## Boundary

Advertising a server global benefits a native Chromium client only if that client binds it and exercises the relevant requests. Exo server coverage and Ozone client coverage remain separate maps. [inferred]

## Feature links

- [protocols/coverage-map](../03-wayland-protocols/README.md)
- [gestures/touchpad-pinch](../06-features/gestures/touchpad-pinch.md)

## Sources

- S-662084865ea5 · 2026-07-18 · Chromium exo wayland server and ozone client source and equivalence inventory
- S-1f554a1713a5 · 2026-07-18 · Lift-map evaluation and corrections
- S-063bb48a06e4 · 2026-11-09 · Pinned donor interface inspection
- S-2bb5f68bf0ba · 2026-11-09 · Pinned donor interface inspection
- S-2e38f34f8b7b · 2026-11-09 · Pinned donor interface inspection
- S-4111d1fe471f · 2026-11-09 · Pinned donor interface inspection
- S-4c46e40f55d9 · 2026-11-09 · Pinned donor interface inspection
- S-a73d64dc0796 · 2026-11-09 · Pinned donor interface inspection
- S-d9193550e1ab · 2026-11-09 · Pinned donor interface inspection
- S-f325935ceeea · 2026-11-09 · Pinned donor interface inspection
- S-c2596d00b100 · 2026-11-09 pin check · Pinned desktop portal contract
- S-76ba87eac499 · 2026-11-09 · Ozone pointer-gesture wrapper declaration and checked listener scope
