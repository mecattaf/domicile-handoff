---
id: input/scroll-feel-and-momentum
title: Finger scrolling and post-lift momentum
kind: feature
family: input
layers: [engine:input, engine:blink, engine:browser, compositor, protocol, config]
domicile_status: landed
domicile_refs: [cprussin/domicile#1912, cprussin/domicile#1930, cprussin/domicile#1943]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: true
scroll_parity: baseline
donors: [sway, scroll, macos]
rows: []
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:input, status:landed, evidence:source, scroll-parity:baseline]
sources: [S-069533fa6b43, S-09fd0ca98c14, S-221352e51e02, S-2b285c11dec3, S-3a58eb504072, S-3f21f7edd27c, S-5ac26707cdda, S-625c4e474be6, S-7460a61a430c, S-7ca0634a8080, S-800e53f29a05, S-8ac9f67b9d70, S-9209061e1959, S-a89a4c46b8f9, S-a90d2490bf23, S-a92c840fc324, S-aae42b458e98, S-aee6e43f49ef, S-b056064c5b44, S-baadedc7c85c, S-c1fcb339d2ef, S-c43367a6e68a, S-cd9abf107411, S-e92a22a88dc7, S-eb1fc0ddd4c7, S-f7db7c7f244b]
---
# Finger scrolling and post-lift momentum

A finger scroll retains its source and independent axis stops. Browser pages use Chromium fling physics after release; native Wayland windows receive physical axes and run their own momentum. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_app_element.cc#L451) [source]

## Behavior

- Wheel notches, high-resolution ticks, finger motion, continuous motion and axis termination remain distinct. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_app_element.cc#L464) [source]
- The engine starts a page fling before the native stop ends its wheel target. A slow release also emits a zero-velocity fling boundary. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0190-domicile-a-flick-s-glide-starts-before-the-scroll-stop.patch#L39) [source]
- Native-window default handling discards synthetic momentum wheels, so the client does not combine its own glide with the page fling. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_app_element.cc#L459) [source]

- A native window receives retained native-axis distance when available. Otherwise the element converts DOM deltas according to their delta mode. Value120 comes separately from normalized wheel ticks. [source] [Native wheel conversion](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_app_element.cc#L451)

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Forwards finger-source pointer axes and stops | Clients retain their own scroll physics | [source](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/cursor.c) |
| scroll | Keeps sway native input semantics and adds layout gestures separately | Workspace motion is not the browser page fling | [source](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/seatop_default.c#L1145) |
| macOS / AppKit, documentation checked 2026-11-09 | Finger phase, momentum phase and precise-delta flag are separate | Momentum stays attached to the target selected at the flick; pointer motion does not retarget it | [Finger phase](https://developer.apple.com/documentation/appkit/nsevent/phase-swift.property) · [Momentum phase](https://developer.apple.com/documentation/appkit/nsevent/momentumphase) · [Delta precision](https://developer.apple.com/documentation/appkit/nsevent/hasprecisescrollingdeltas) [docs] |

## What the notes found

- The historical checkpoint identifies a missing velocity-to-fling producer as a source hypothesis. Its page comparison did not complete, so it establishes no measured page-latency or momentum result. S-8ac9f67b9d70 · 2026-11-06 · Native scroll and fling boundary study [docs]
- The retained distinction is between source conversion, gain, event cadence, post-release motion and presentation latency. Those quantities do not substitute for one another. [docs]
- 2026-11-09: The pin retains finger-source and axis-stop metadata at the native-window boundary and discards synthetic momentum wheels there. Historical missing-metadata symptoms are superseded by the merged input stack; physical pointer behavior remains separately unqualified [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_app_element.cc#L451> <https://github.com/cprussin/domicile/pull/1930> <https://github.com/cprussin/domicile/pull/1943>
- 2026-11-09: The libinput route retains scaled linear native-axis distance alongside accelerated browser deltas. Disabling its converter emits any still-owed axis stops, clears accelerator state and releases its recorded pressed buttons [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0174-domicile-preserve-native-scroll-sources-and-axis-stops.patch#L472> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0175-domicile-share-libinput-devices-and-native-typing.patch#L868> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/active_scroll_axes.h#L17>
- 2026-11-09: The finger-scroll accelerator derives gain from vector speed, clamps it between one and four, and resets its speed interval after a gap over 100 ms. Explicit configuration controls acceleration and the separate scroll factor [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/scroll_accelerator.cc#L20> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0173-domicile-configure-touchpads-and-mice-during-a-session.patch#L322>
- 2026-11-09: Accelerated finger scrolling emits whole units and carries fractional remainders per axis. Disabling acceleration preserves fractional linear distance. A stopped axis clears its remainder; the speed timestamp resets only after both axes become inactive [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/scroll_accelerator.cc#L55> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/scroll_accelerator.cc#L86>
- 2026-11-09: The wheel helper rounds negated detent counts multiplied by Chromium’s wheel unit. A separate axis helper multiplies supplied distance by its factor and clamps the product to the integer range [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/wheel_ticks.cc#L11> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/scroll_factor.h#L13>
- 2026-11-09: At the pin the compositor maps wheel, finger, continuous and wheel-tilt axis sources, and engine patch 'preserve native scroll sources and axis stops' is in the series [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/client_requests.rs#L377> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0174-domicile-preserve-native-scroll-sources-and-axis-stops.patch>
- 2026-11-09: Native-app wheel forwarding prefers retained native-axis distance; without it, the element rescales DOM deltas by delta mode. Value120 derives separately from normalized wheel ticks, and synthetic momentum wheels are rejected [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_app_element.cc#L451>
- 2026-11-05: Otto's pointer handler forwards axis source, high-resolution wheel steps, per-axis relative direction, finger stops and frame boundaries to Wayland clients (otto) [source] <https://github.com/nongio/otto/blob/1c148c1bbc9f80eea415fb695c0785ab28d4be35/src/input/pointer.rs#L886>
- 2026-11-08: The pinned engine series contains a flick-glide patch that measures speed over the last 100 ms before lift and keeps a page gliding, matching the embedded-tab gap seen on 2026-11-06 [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0189-domicile-a-two-finger-flick-on-a-page-keeps-gliding.patch>
- 2026-11-05: On a 2026-11-05 main snapshot the compositor labeled every forwarded scroll as wheel input and the engine relay carried distances and v120 steps with no finger origin or axis stop [source]
- 2026-11-06: External Chrome kept gliding after finger lift on a touchpad while a page in an embedded browser tab stopped at lift-off; raw input sent scroll updates and a stop without a fling producer [measured] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0189-domicile-a-two-finger-flick-on-a-page-keeps-gliding.patch>
- 2026-11-06: Chrome converts Wayland scroll to pixels at a measured factor of 12; a Domicile configuration requesting factor 1 with acceleration off was loaded, and no second 12x stage inside Domicile was found. Scroll distance can still differ between an embedded window and a Chrome window [measured] <https://github.com/cprussin/domicile/pull/1884#issuecomment-6705916355>
- 2026-11-06: A disposable-guest comparison of external Chrome and an embedded tab under identical synthetic two-finger input stopped before comparison on 2026-11-06; native axis, release and stop events were captured, momentum was not measured [measured]
- 2026-11-07: Under a test compositor, Chrome 156 turned finger-axis values 1, 2.5 and 10 into page wheel deltas of 12, 30 and 120 pixels; a direct one-pixel control stayed at one pixel. [measured]
- 2026-11-07: Between the 2026-11-06 checked build and main of 2026-11-07 the input settings and conversion code were unchanged (scroll factor 1, acceleration off); scroll distance differed between embedded pages and Chrome windows rather than over time. [measured]

## Where it touches Domicile

- `engine:input` estimates release velocity over a recent 100 ms sample window and yields zero after a resting lift. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/fling_tracker.cc#L10) [source]
- `engine:blink` suppresses momentum wheels at a native-window boundary while retaining source/stop data. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_app_element.cc#L455) [source]
- Status at the pin: landed. [#1912](https://github.com/cprussin/domicile/pull/1912) supplies native flick momentum; [#1930](https://github.com/cprussin/domicile/pull/1930) and [#1943](https://github.com/cprussin/domicile/pull/1943) settle the page/native distinction and ordering. Patch “domicile: a flick's glide starts before the scroll stop” is present at the pin. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0190-domicile-a-flick-s-glide-starts-before-the-scroll-stop.patch#L4) [source]

- The pinned gesture contract documents finger-source axes and axis stops. The fling producer and page/native momentum handling are established separately by the source and merge references above. [docs] [Native scroll contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L88)

## Lineage

- 2026-11-06: the bounded scroll study records source hypotheses and incomplete comparisons, retaining their limits. S-8ac9f67b9d70 · 2026-11-06 · Native scroll and fling boundary study [docs]
- 2026-11-09: the pinned input route orders fling before stop and keeps page momentum out of native client delivery. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_app_element.cc#L459) [source]

## Sources

- S-7ca0634a8080 · 2026-11-09 · Sway pointer motion, source and cursor routing.

- S-a90d2490bf23 · 2026-11-09 · Native gesture and scroll contract.

- S-eb1fc0ddd4c7 · 2026-11-09 · AppKit precise scrolling delta contract.

- S-f7db7c7f244b · 2026-11-09 · AppKit momentum phase and target contract.

- S-3f21f7edd27c · 2026-11-09 · AppKit finger phase and gesture target contract.

- S-221352e51e02 · 2026-11-09 · Native release velocity estimation.
- S-2b285c11dec3 · 2026-11-09 · Engine patch ordering page fling before native scroll termination.
- S-800e53f29a05 · 2026-11-06 · Native input source audit.
- S-8ac9f67b9d70 · 2026-11-06 · Native scroll and fling boundary study.
- S-a92c840fc324 · 2026-11-09 · Native application event defaults and momentum boundary.
