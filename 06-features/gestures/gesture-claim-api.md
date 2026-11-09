---
id: gestures/gesture-claim-api
title: Gesture phases and ownership
kind: feature
family: gestures
layers: [engine:input, engine:browser, sdk, manganese]
domicile_status: landed-with-gap
domicile_refs: [cprussin/domicile#1913, cprussin/domicile#1928, cprussin/domicile#1941]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: true
scroll_parity: baseline
donors: [sway, scroll, niri, kde, ash, fusuma, libinput-gestures]
rows: []
donor_availability: chromeos-gated
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:gestures, status:landed-with-gap, evidence:source, scroll-parity:baseline]
sources: [S-0a6357b8c6cf, S-1e7caee5f14c, S-284acbba85b2, S-34c3080edb40, S-37152ce70954, S-47ef16399888, S-5599731427c4, S-774e974df80c, S-7a88fa265b49, S-866b9f3f829d, S-9f8ae1b1c71c, S-a90d2490bf23, S-b1a9c0db3aa5, S-bf63be5a1d74, S-cb66a79f1ebd, S-d2242e1a7d7f, S-d68dcb7e6552, S-e331d31605d8]
---
# Gesture phases and ownership

A shell claims a gesture by kind and count, then receives begin, updates and end. Recognition and action selection stay separate. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L31) [docs]

## Behavior

- The event includes phase, kind, fingers, delta, absolute scale, incremental rotation and cancellation. An edge gesture also identifies a side. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/gestures.ts#L9) [source]
- The engine selects ownership at begin. Removing a grab does not transfer an in-flight sequence to a different page. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/desk_gestures.cc#L56) [source]
- An explicit canceled end terminates the active native sequence. Matching finger and edge metadata remain those captured at begin. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/domicile/native_gesture_sink.cc#L28) [source]

- Shell gesture events omit acquisition device identifiers and timestamps. Touchpad deltas use accelerated pointer units; edge gestures use display pixels. Native pinch has a separate payload retaining device and time. [source] [Native gesture payload](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/types/native_gesture.h#L21) · [Native pinch payload](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/types/native_pinch.h#L15)

- SDK completion matching favors explicit finger or edge selectors over wildcards, then a binding requesting more directions. A swipe gains a diagonal direction when minor-axis travel reaches half the dominant-axis travel. [source] [Matching specificity](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L203) · [Direction classification](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L225)

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Begin-time gesture matching | End-time direction chooses the action | [source](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/seatop_default.c#L808) |
| scroll | Continuous deltas coexist with discrete bindings | Layout motion can follow updates without inventing a new gesture transport | [source](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/seatop_default.c#L1145) |
| niri | Shell recognition and native delivery have separate state | Native delta and rotation are forwarded; cumulative scale is raised to the product of configured input and window sensitivity, both defaulting to 1 | [source](https://github.com/YaLTeR/niri/blob/9955d921ce6491e34fb74ec94f30a7cca76826e4/src/input/mod.rs#L4478) |
| KDE KWin | Recognizer cancellation follows gesture unregistration | Only candidates reaching their threshold trigger at completion | [source](https://github.com/KDE/kwin/blob/0406762df7794213f12ce87bc8f6fd322dcdc53c/src/gestures.cpp#L83) · [source](https://github.com/KDE/kwin/blob/0406762df7794213f12ce87bc8f6fd322dcdc53c/src/gestures.cpp#L240) |
| Ash @ Chromium 150 | Continuous overview recognition owns its remainder | Completed-gesture overscroll is consumed; concrete actions use ChromeOS controllers | [source](https://github.com/chromium/chromium/blob/150.0.7871.124/ash/wm/gestures/wm_gesture_handler.cc#L98) · [source](https://github.com/chromium/chromium/blob/150.0.7871.124/ash/wm/gestures/wm_gesture_handler.cc#L159) |
| Fusuma @ 3.12.0 | Phase-indexed repeats and thresholded one-shot actions | Recent movement selects an action rather than preserving a native Wayland stream | [source](https://github.com/iberianpig/fusuma/blob/v3.12.0/lib/fusuma/plugin/detectors/swipe_detector.rb#L20) · [source](https://github.com/iberianpig/fusuma/blob/v3.12.0/lib/fusuma/plugin/detectors/swipe_detector.rb#L59) |
| libinput-gestures @ 2.81 | Accumulated swipe displacement selects an action at end | Exact finger counts precede fallback; cancellation dispatches no action | [source](https://github.com/bulletmark/libinput-gestures/blob/2.81/libinput-gestures#L480) · [source](https://github.com/bulletmark/libinput-gestures/blob/2.81/libinput-gestures#L534) · [source](https://github.com/bulletmark/libinput-gestures/blob/2.81/libinput-gestures#L998) |

## What the notes found

- The reference mechanisms support two consumers of the same lifetime: continuous motion uses updates, while a discrete binding uses the final direction. [docs]
- A direction is unavailable at begin, so acquisition ownership is keyed by kind and count. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L134) [docs]
- 2026-11-07: A probe reproduced a second sender's begin for an unfocused app canceling another client's active gesture, covered by a regression test. Raw phase delivery and begin-time ownership were already covered by #1913 [measured] <https://github.com/cprussin/domicile/pull/1913>
- 2026-11-07: The maintainer chose libinput for gesture recognition so the repository hosts no gesture engine; edge and corner gestures require an engine-side recognizer, since libinput reports neither touchscreen gestures nor touchpad finger position [docs] <https://github.com/cprussin/domicile/issues/1908>
- 2026-11-07: Domicile reads input devices only through logind, so a fusuma-style helper that opens devices itself is not available; bindings live in shell properties beside key bindings, as the maintainer's guidelines state [docs] <https://github.com/cprussin/domicile/issues/1908>
- 2026-11-07: A prototype matched bindings when a gesture began and owned the sequence: unmatched sequences reached the client unchanged and continuous deltas were forwarded unchanged. The existing app forwarding path runs after page delivery, so it cannot reserve gestures over shell DOM or embedded pages [docs]
- 2026-11-07: A bindings review of a prototype arbitration seam found that a compositor round trip per gesture phase requires defined ordering, disconnect and latency behavior, that disposition replies belong to the originating connection, and that locked input receives a terminal consume response [inferred]

## Where it touches Domicile

- `engine:input` owns the sequence before content dispatch. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/domicile/native_gesture_sink.cc#L28) [source]
- `engine:browser` remembers the winning channel. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/desk_gestures.cc#L68) [source]
- `sdk` exposes grab/ungrab and typed event subscription; matching shares the shell keymap modes. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/gestures.ts#L42) [source]
- Status at the pin: landed-with-gap. [#1913](https://github.com/cprussin/domicile/pull/1913) answers continuous phases and completion matching, and [#1928](https://github.com/cprussin/domicile/pull/1928) answers unbound swipe/hold delivery. Shortcut-inhibited native delivery of a grabbed sequence is not addressed at the pin. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L144) [docs]

## Lineage

- 2026-11-07: donor research isolated acquisition, phase ownership and shell action recognition. S-d68dcb7e6552 · 2026-11-07 · Native gesture donor mechanisms [docs]
- 2026-11-09: the pinned SDK exposes typed phases and begin-time grabs. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/gestures.ts#L14) [source]

## Sources

- S-34c3080edb40 · 2026-11-09 · Gesture daemon end-time matching.

- S-47ef16399888 · 2026-11-09 · Fusuma swipe action recognition.

- S-5599731427c4 · 2026-11-09 · Ash continuous gesture ownership.

- S-0a6357b8c6cf · 2026-11-09 · KWin recognizer lifetime and completion.

- S-e331d31605d8 · 2026-11-09 · Niri input ownership and pinch sensitivity.

- S-cb66a79f1ebd · 2026-11-09 · Native shell gesture payload.
- S-bf63be5a1d74 · 2026-11-09 · Native pinch acquisition payload.

- S-1e7caee5f14c · 2026-11-09 · Typed gesture subscription and ownership API.
- S-774e974df80c · 2026-11-09 · Browser gesture channel ownership and unbound forwarding.
- S-7a88fa265b49 · 2026-11-09 · Native gesture lifetime and cancellation.
- S-a90d2490bf23 · 2026-11-09 · Native gestures, page defaults and shell binding contract.
- S-d2242e1a7d7f · 2026-11-09 · SDK gesture grammar and completion matching.
- S-d68dcb7e6552 · 2026-11-07 · Native gesture donor mechanisms.
