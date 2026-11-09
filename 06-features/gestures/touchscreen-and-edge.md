---
id: gestures/touchscreen-and-edge
title: Touchscreen contacts and display-edge swipes
kind: feature
family: gestures
layers: [engine:input, engine:blink, engine:browser, compositor, protocol, sdk, manganese]
domicile_status: landed
domicile_refs: [cprussin/domicile#1924, cprussin/domicile#1925]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: true
scroll_parity: baseline
donors: [smithay, lisgd, fusuma]
rows: [wl_touch]
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:gestures, status:landed, evidence:source, scroll-parity:baseline]
sources: [S-09fd0ca98c14, S-28d96b74fa7b, S-33889728083c, S-49e002eded8c, S-625c4e474be6, S-6d51bf619041, S-866b9f3f829d, S-900dfbb65723, S-a89a4c46b8f9, S-a90d2490bf23, S-af786326abd1, S-b056064c5b44, S-b1087a2fab28, S-bddbf79bec12, S-c40e302fa0da, S-d68dcb7e6552, S-e5e4d7fe8e6a, S-fd0c87388c02]
---
# Touchscreen contacts and display-edge swipes

Direct contacts reach the touched Wayland window. A bound display-edge swipe is decided before those contacts reach the desktop page. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L35) [docs]

## Behavior

- The seat advertises touch permanently. A client that has bound touch receives contacts; another receives a primary-finger pointer press and motion. The choice remains fixed for that contact sequence. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L51) [docs]
- The first touch focuses a native window. Window replacement, removal, locking, input capture and connection loss cancel active contacts. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L49) [docs]
- On a tty, a single finger beginning within 3% of a grabbed display edge can become an edge gesture after 6% inward travel. A tap, second finger, timeout or rejected direction releases the buffered contacts in order. Nested edges belong to the host. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L58) [docs]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| Smithay Anvil | Direct contact handlers retain slots, focus and frame boundaries | Touch transport differs from repeating pointer button presses | [source](https://github.com/Smithay/smithay/blob/65aa86fa5236e41775c313408102a5d5c2d1e2ca/anvil/src/input_handler.rs#L243) |
| lisgd | Recognizes touchscreen slot motion in transformed coordinates | Origin, displacement, orientation and timeout participate in edge matching | S-d68dcb7e6552 · 2026-11-07 · Native gesture donor mechanisms |
| Fusuma | Touchpad recognizers use gesture phases, thresholds and timers | Its recognizers do not provide raw touchscreen contacts | S-d68dcb7e6552 · 2026-11-07 · Native gesture donor mechanisms |
| lisgd @ 0.4.0 | Transforms contacts before displacement, direction and edge matching | An action can fire during motion or at release, subject to timeout | [source](https://git.sr.ht/~mil/lisgd/tree/0.4.0/item/lisgd.c#L178) · [source](https://git.sr.ht/~mil/lisgd/tree/0.4.0/item/lisgd.c#L295) · [source](https://git.sr.ht/~mil/lisgd/tree/0.4.0/item/lisgd.c#L329) |

## What the notes found

- Touchpad gestures contain device-recognized motion; touchscreens contain positioned contacts. One route cannot supply geometry that the other does not report. [docs]
- Completed-frame cancellation is a separate delivery concern. The pinned contract compensates for Smithay by refreshing active points before cancellation. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L55) [docs]
- 2026-11-09: Aura selects pressed-handler, capture, pinch-handler or touch-locked targets before ordinary located-event traversal. Touch target locking therefore has a path distinct from geometric subtree selection (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window_targeter.cc#L77> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/aura/window_targeter.cc#L116>
- 2026-11-09: At the pin the compositor calls add_touch on its seat, and the EIS translation layer still documents the touchless-seat fallback of a single emulated left button [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L867> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/translation.rs#L14>
- 2026-11-08: The pinned compositor carries a touch module and a seat that handles touch, so the 2026-11-06 contact-forwarding gap has a counterpart in source at the pin [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/touch.rs> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/seat.rs>
- 2026-11-05: On a 2026-11-05 main snapshot the seat advertised keyboard and pointer only; the app element folded pointer events into mouse events, losing contact identity, while per-device touchscreen-to-display mapping already existed (#1344) [source] <https://github.com/cprussin/domicile/pull/1344>
- 2026-11-07: In the pinned Smithay fork, canceling after a completed touch frame skipped the active contacts, so lock, shell reload or device cancellation could not end a touch sequence; a fix passed the library's 50 tests (smithay@fb981077def1405300e69a5993e9789768e772aa) [measured] <https://github.com/cprussin/smithay/pull/1>
- 2026-11-06: On a two-touchscreen laptop, one-finger scrolling did not work and a two-finger pinch zoomed the shell page on both panels together instead of the focused Chrome content (2026-11-06) [measured]
- 2026-11-07: At a 2026-11-07 main snapshot, two touchscreen contacts over a native app element became two left-button presses; when Chromium canceled the contacts, no releases were forwarded, which can leave a native client with a held mouse button [measured]
- 2026-11-07: Smithay's default touch grab sent every simultaneous contact to the first app touched on a two-touchscreen laptop; keeping each contact on the target the engine selected is covered by a two-app regression test [measured]
- 2026-11-07: A native client that never binds wl_touch receives no touch input from a contact-only path; [#1924](https://github.com/cprussin/domicile/pull/1924) adds a pointer fallback for those clients [docs]
- 2026-11-09: L-31 records contact release during touchscreen cancellation after a completed frame. The pinned contract refreshes contact positions before cancellation; #1924 is merged (Domicile@pin) [docs] <https://github.com/cprussin/domicile/pull/1924> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L55>
- 2026-11-08: By 2026-11-08 the maintainer had merged or opened #1924 (touchscreen contacts to native windows) and #1925 (edge swipes) after the 2026-11-07 gesture issue [docs] <https://github.com/cprussin/domicile/pull/1924> <https://github.com/cprussin/domicile/pull/1925>
- 2026-11-04: At the 2026-11-04 snapshot input.pointers rebinds tablets, touchscreens and absolute mice to outputs, the equivalent of sway map_to_output; touch reaches native clients as a one-finger left-button pointer, with no wl_touch [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-CONFIG.md#L135> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/A-HEADLESS-DESKTOP.md#L53>
- 2026-11-05: niri's per-device touch and tablet configuration PR names devices alongside global defaults and was tested by its author on a laptop with two touchscreens; Domicile's #1344 supplies the equivalent mapping (niri) [docs] <https://github.com/niri-wm/niri/pull/1856> <https://github.com/cprussin/domicile/pull/1344>
- 2026-11-06: A touchscreen finger on a native window arrived as pointer input rather than a touch contact (2026-11-06 snapshot, before #1924) [docs] <https://github.com/cprussin/domicile/pull/1924>

## Where it touches Domicile

- `engine:input` buffers per-device edge candidates and replays a rejected prefix. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/touch_edge_swipes.cc#L58) [source]
- `engine:input` chooses a grabbed nearby edge, rejects a second contact during arbitration and reports begin/update/end. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/domicile/edge_swipe.cc#L19) [source]
- `engine:blink` excludes native-window touch from desktop pan/zoom; `compositor` owns native touch and pointer fallback. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L45) [docs]
- Status at the pin: landed. [#1924](https://github.com/cprussin/domicile/pull/1924) answers permanent touch advertisement with pointer fallback; [#1925](https://github.com/cprussin/domicile/pull/1925) carries edge arbitration. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L150) [docs]

## Lineage

- 2026-11-07: source comparisons treated contact cancellation and early edge arbitration as independent mechanisms. S-d68dcb7e6552 · 2026-11-07 · Native gesture donor mechanisms [docs]
- 2026-11-09: the pinned contract records permanent touch support, fallback and tty edge replay. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L35) [docs]

## Sources

- S-49e002eded8c · 2026-11-09 · Touchscreen gesture geometry and action timing.

- S-28d96b74fa7b · 2026-11-09 · Display-edge contact withholding and replay.
- S-a90d2490bf23 · 2026-11-09 · Native gestures, page defaults and shell binding contract.
- S-b1087a2fab28 · 2026-11-07 · Completed-frame touch cancellation source finding.
- S-bddbf79bec12 · 2026-11-09 · Display-edge gesture recognition and cancellation.
- S-d68dcb7e6552 · 2026-11-07 · Native gesture donor mechanisms.
