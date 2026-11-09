---
id: gestures/touchpad-pinch
title: Touchpad pinch to native windows and browser pages
kind: feature
family: gestures
layers: [engine:input, engine:blink, engine:browser, compositor, protocol]
domicile_status: landed
domicile_refs: [cprussin/domicile#1911, cprussin/domicile#1927, cprussin/domicile#1944]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: true
scroll_parity: baseline
donors: [sway, niri, smithay, libinput-gestures]
rows: []
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:gestures, status:landed, evidence:source, scroll-parity:baseline]
sources: [S-09fd0ca98c14, S-0f7230f8f6a6, S-33889728083c, S-34c3080edb40, S-3e3acf139049, S-7625723017ab, S-774e974df80c, S-7df3a46fc2ab, S-9b695076c4b3, S-a89a4c46b8f9, S-a90d2490bf23, S-a92c840fc324, S-af786326abd1, S-b056064c5b44, S-d68dcb7e6552, S-d9193550e1ab, S-e331d31605d8]
---
# Touchpad pinch to native windows and browser pages

A pinch retains its native phases until a shell claims it, a Wayland client receives it, or a browser page uses it for page zoom. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L23) [docs]

## Behavior

- Two fingers remain available to the window. A shell may grab a pinch of three or more fingers by kind and count before its direction is known. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L123) [docs]
- Native delivery retains absolute scale, incremental rotation, timestamps, finger count and cancellation. Pointer departure, locking, window removal and connection loss terminate the sequence. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L25) [docs]
- An unclaimed browser-window pinch changes page zoom and retains that zoom for the site. The desktop root does not scale. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L128) [docs]

- A begin validates the pointer window and target surface before replacing an active pinch. Later phases must match its owner, device and window. [source] [Pinned admission and lifetime checks](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/pinch.rs#L35)

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | `bindgesture` owns matching sequences; unmatched events remain native | Direction resolves at completion; cancellation executes no command | [source](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/seatop_default.c#L808) |
| niri | Native pinch is distinct from shell swipe recognition | Delta and rotation are forwarded; configurable input and window sensitivity exponentiate cumulative scale, with both defaults equal to 1 | [source](https://github.com/YaLTeR/niri/blob/9955d921ce6491e34fb74ec94f30a7cca76826e4/src/input/mod.rs#L4478) |
| libinput-gestures @ 2.81 | Accumulates reported scale minus one and rotation increments | That end-time recognition metric differs from native cumulative-scale transport | [source](https://github.com/bulletmark/libinput-gestures/blob/2.81/libinput-gestures#L566) |

## What the notes found

- The reference review distinguishes metadata transport from application behavior: a client receiving a pinch does not prove that client binds it to zoom. [docs]
- Scale, target selection and physical-device behavior are separate observations. Local component coverage is not hardware qualification. [docs]
- 2026-11-09: GTK 4.20.0 forwards the received native pinch scale into its toolkit event and converts rotation from degrees to radians. Wayland’s canceled flag selects canceled versus ended gesture phases (gtk@4.20.0) [source] <https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L2025> <https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L2086> <https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L2106>
- 2026-11-09: Blink's entering-fullscreen path installs unit page-scale constraints and resets a local main frame to one; exit clears those fullscreen constraints. Domicile guest pinch updates HostZoomMap browser zoom through a separate handler (chromium@2493812cc0621330895498ea437bde415944c203) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/third_party/blink/renderer/core/frame/fullscreen_controller.cc#L134> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/third_party/blink/renderer/core/frame/fullscreen_controller.cc#L441> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/web_view_guest.cc#L1554>
- 2026-11-05: On DRM at Chromium 159, the libinput converter handled motion, button and axis events with no pinch, swipe or hold case, so those events never reached Blink or Smithay; Chromium's pinch path delivers cancelable ctrl+wheel to pages [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/events/ozone/evdev/libinput_event_converter.cc#L350> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/events/ozone/evdev/event_factory_evdev.cc#L376> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/input/touchpad_pinch_event_queue.cc#L48>
- 2026-11-08: The pinned engine series carries patches preserving native touchpad pinches through app input and zooming the window under the pointer, and the compositor has a pinch module [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0184-domicile-preserve-native-touchpad-pinches.patch> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0191-domicile-a-touchpad-pinch-zooms-the-window-under-it-.patch> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/pinch.rs>
- 2026-11-09: The Ozone Wayland wrapper registers pinch and hold listeners; no swipe object or listener appears in the checked wrapper files (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_zwp_pointer_gestures.cc#L72> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_zwp_pointer_gestures.cc#L87>
- 2026-11-09: Qt Wayland 6.9.0’s gesture-enabled pinch path retains the begin surface and finger count. Each update converts the received scale into its difference from the previous update before dispatching the toolkit gesture (qtwayland@6.9.0) [source] <https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandpointergestures.cpp#L128> <https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandpointergestures.cpp#L153>
- 2026-11-09: At the pin the engine series includes patches 'preserve native touchpad pinches' and 'the shell binds touchpad gestures of three' fingers or more, and the converter carries a HandleShellGesture entry point [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0184-domicile-preserve-native-touchpad-pinches.patch> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0185-domicile-the-shell-binds-touchpad-gestures-of-three-.patch>
- 2026-11-07: At a 2026-11-07 main snapshot, the engine's libinput adapter dropped touchpad pinch events, so a pinch over a native Chrome app window left the client at scale 1.0 while the shell page zoomed to 1.5 [measured] <https://github.com/cprussin/domicile/pull/1907>
- 2026-11-07: A compositor regression showed native Wayland pinch delivery (begin, update, cancel with scale and rotation) without invented scroll events; cancel on app removal and a fresh pinch afterwards also passed [measured] <https://github.com/cprussin/domicile/pull/1907>
- 2026-11-07: Forwarding alone still zoomed the shell: the engine's passive document wheel listener let the browser apply zoom. An explicit non-passive wheel listener on native-window regions of the app element made the client the sole zoom target [measured] <https://github.com/cprussin/domicile/pull/1907>
- 2026-11-07: Chromium's pinch acknowledgment queue lost the consumed state when the app element was removed mid-gesture, so later updates scaled the shell to about 1.17. Keeping consumed state until end or cancel, with a fresh target per gesture, fixed it [measured] <https://github.com/cprussin/domicile/pull/1907>
- 2026-11-07: The Rust wire parser accepted a pinch begin with zero fingers and zero or negative scales that the TypeScript schema rejected; parsing into a non-zero integer and rejecting invalid scales aligned the two [measured] <https://github.com/cprussin/domicile/pull/1907>
- 2026-11-06: On a Magic Trackpad, two-finger back/forward worked but pinch-to-zoom did not on the 2026-11-06 build; its libinput converter dropped gesture events and the compositor exposed no pinch route [measured]
- 2026-11-06: A Wayland test client received pinch, swipe, hold and touch contacts from a compositor-side implementation on 2026-11-06, with cancellation on lock and connection close; the engine-to-compositor physical path was unqualified [docs]
- 2026-11-07: Over an embedded page element, a synthesized trackpad-style pinch scaled the shell from 1 to 1.5 while the guest stayed at 1 and received no events; trusted touch scrolling did reach the guest [measured]

## Where it touches Domicile

- `engine:input` acquires native metadata on the tty and through host Wayland when nested; `engine:blink` forwards a native-window pinch from the default handler. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_app_element.cc#L404) [source]
- `engine:browser` and `compositor` own sequence routing and Wayland delivery; [#1911](https://github.com/cprussin/domicile/pull/1911) and [#1927](https://github.com/cprussin/domicile/pull/1927) carry native pinch, while [#1944](https://github.com/cprussin/domicile/pull/1944) makes the browser guest use page zoom. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L23) [docs]
- Status at the pin: landed. The question of a browser guest scaling the desktop is answered upstream by [#1944](https://github.com/cprussin/domicile/pull/1944). [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L128) [docs]

## Lineage

- 2026-11-06–2026-11-07: native-input work separated typed pinch acquisition, ownership and client delivery. S-d68dcb7e6552 · 2026-11-07 · Native gesture donor mechanisms [docs]
- 2026-11-09: the pinned gesture contract records native-window pinch and browser page zoom as distinct routes. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L23) [docs]

## Sources

- S-a92c840fc324 · 2026-11-09 · Native application input defaults and gesture delivery.

- S-34c3080edb40 · 2026-11-09 · Gesture daemon pinch recognition metric.

- S-e331d31605d8 · 2026-11-09 · Niri pinch sensitivity and native forwarding.

- S-7625723017ab · 2026-11-09 · Native pinch admission and lifetime.

- S-774e974df80c · 2026-11-09 · Browser gesture channel ownership and unbound forwarding.
- S-a90d2490bf23 · 2026-11-09 · Native gestures, page defaults and shell binding contract.
- S-d68dcb7e6552 · 2026-11-07 · Native gesture donor mechanisms.
