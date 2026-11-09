---
id: gestures/multi-finger-swipe-and-hold
title: Multi-finger swipes and holds
kind: feature
family: gestures
layers: [engine:input, engine:browser, compositor, protocol, sdk, manganese]
domicile_status: landed-with-gap
domicile_refs: [cprussin/domicile#1913, cprussin/domicile#1928, cprussin/domicile#1929, cprussin/domicile#1941]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: true
scroll_parity: baseline
donors: [sway, scroll, niri, kde, ash, fusuma, libinput-gestures]
rows: [pointer-gestures-v1]
donor_availability: chromeos-gated
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:gestures, status:landed-with-gap, evidence:source, scroll-parity:baseline]
sources: [S-0a6357b8c6cf, S-21e5e90c662c, S-28be5e927839, S-2bb5f68bf0ba, S-34c3080edb40, S-5599731427c4, S-65d1c77e9380, S-774e974df80c, S-817625bbdcf4, S-866b9f3f829d, S-87a48f59dd5d, S-92faa6ff2e46, S-9f8ae1b1c71c, S-a89a4c46b8f9, S-a90d2490bf23, S-af786326abd1, S-b056064c5b44, S-b21e6b233c1d, S-d2242e1a7d7f, S-d68dcb7e6552, S-e331d31605d8, S-ecfec2f17332, S-f152f6bc05af, S-fd0c87388c02]
---
# Multi-finger swipes and holds

The shell can follow a multi-finger gesture continuously and run a binding when its final direction is known. Unclaimed swipes and holds remain available to a native client. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L31) [docs]

## Behavior

- A grab matches kind and finger count at begin. The page that obtains ownership receives the sequence through end, even if the pointer moves. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/desk_gestures.cc#L63) [source]
- Bindings share keybinding modes. Finger-count specificity wins over a wildcard; directional specificity resolves the remaining matches. Canceled endings run no action. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L143) [source]
- An unbound swipe or hold of at least three fingers targets the native window beneath the pointer at begin. One- and two-finger holds stay inside the engine. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L138) [docs]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Gesture matching and ownership in the seat operation | Bound gestures are consumed; unmatched phases continue to clients | [source](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/seatop_default.c#L808) |
| scroll | Discrete matching precedes continuous layout scrolling | Continuous motion applies sensitivity and layout scale; otherwise native forwarding | [source](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/seatop_default.c#L1145) |
| niri | Unlocked three-finger swipes enter layout motion and four-finger swipes enter the overview | Other eligible swipes can reach Smithay; UI state can suppress starts, and hold has its own forwarding path | [source](https://github.com/YaLTeR/niri/blob/9955d921ce6491e34fb74ec94f30a7cca76826e4/src/input/mod.rs#L4253) |
| KDE KWin | Finger count admits candidates; accumulated motion locks an axis | Opposite-direction candidates are canceled while survivors receive progress | [source](https://github.com/KDE/kwin/blob/0406762df7794213f12ce87bc8f6fd322dcdc53c/src/gestures.cpp#L117) · [source](https://github.com/KDE/kwin/blob/0406762df7794213f12ce87bc8f6fd322dcdc53c/src/gestures.cpp#L154) |
| Ash @ Chromium 150 | Fling-cancel and fling-start frame a three- or four-finger sequence | Deltas accumulate and a finger-count change resets the recognizer; concrete actions use ChromeOS controllers | [source](https://github.com/chromium/chromium/blob/150.0.7871.124/ash/wm/gestures/wm_gesture_handler.cc#L159) · [source](https://github.com/chromium/chromium/blob/150.0.7871.124/ash/wm/gestures/wm_gesture_handler.cc#L187) |
| Fusuma @ 3.12.0 | Hold recognition combines gesture phases and timer updates | Its one-shot action can fire while fingers remain held | [source](https://github.com/iberianpig/fusuma/blob/v3.12.0/lib/fusuma/plugin/detectors/hold_detector.rb#L28) · [source](https://github.com/iberianpig/fusuma/blob/v3.12.0/lib/fusuma/plugin/detectors/hold_detector.rb#L59) |
| libinput-gestures @ 2.81 | Chooses a hold command when fingers are released | It selects the largest configured delay reached by elapsed time | [source](https://github.com/bulletmark/libinput-gestures/blob/2.81/libinput-gestures#L603) · [source](https://github.com/bulletmark/libinput-gestures/blob/2.81/libinput-gestures#L630) |

## What the notes found

- The donor comparison separates an event stream from the command it eventually triggers. A completed gesture binding and a finger-following animation can use the same phases. [docs]
- Scroll maps deltas through sensitivity and layout scale. That is a shell transform rather than a universal physical-pixel equality. [docs]
- 2026-11-09: During ChromeOS Ash cycling, two- or three-finger scrolling steps only after horizontal movement dominates and crosses a threshold. Finger-count changes reset accumulation; two-finger direction compensates for natural-scroll settings (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_cycle/window_cycle_event_filter.cc#L126> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_cycle/window_cycle_event_filter.cc#L300> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/window_cycle/window_cycle_event_filter.cc#L335>
- 2026-11-09: Exo still leaves its pointer swipe request unimplemented at this engine pin; its pinch delegate sends real begin, update and end events (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/zwp_pointer_gestures.cc#L30> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/zwp_pointer_gestures.cc#L52>
- 2026-11-05: sway resolves competing gesture bindings by input device first, then exact matches, then the most matching directions; a --exact flag prefers exact matches. [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L569-L575>
- 2026-11-09: scroll exposes gesture_scroll_enable (default true), gesture_scroll_fingers (default 3) and gesture_scroll_sensitivity (default 1.0, negative sign flips natural versus reverse), plus a separate mouse sensitivity setting (scroll@b6586d2) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L233-L248>
- 2026-11-07: At a 2026-11-07 main snapshot, touchpad swipe and hold events stopped in the engine's libinput adapter, and a regression showed swipe and hold phases not reaching a native test client; libinput reported the capability on both ELAN touchpad interfaces and a Magic Trackpad [measured]
- 2026-11-05: Four external recognizers were surveyed on 2026-11-05: Swipe (touchscreen and touchpad direction), wzmach (Rust swipe, pinch, shear, rotation, hold), fusuma (begin, update, end, cancel, sensitivity) and lisgd (touchscreen edges, finger counts) (lisgd) [docs] <https://github.com/iberianpig/fusuma> <https://github.com/Ryu-ga/lisgd> <https://github.com/maurges/wzmach> <https://github.com/evuraan/Swipe>
- 2026-11-08: As of 2026-11-08 the maintainer had landed or opened #1913 (shell bindings for 3+ finger swipe, pinch and hold using bindgesture syntax), #1928 (forward unused swipes and holds to the window under the pointer) and #1929 [docs] <https://github.com/cprussin/domicile/pull/1913> <https://github.com/cprussin/domicile/pull/1928> <https://github.com/cprussin/domicile/pull/1929>
- 2026-11-09: L-25 tracks unbound swipe and hold phases. Merged #1928 and the pinned gesture guide forward shell-unclaimed gestures to the native client selected at begin (Domicile@pin) [docs] <https://github.com/cprussin/domicile/pull/1928> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md>
- 2026-11-05: Solium routes touchscreen contacts with ownership and cancellation and builds multi-point Qt events; its touchpad gesture events stop in its own input match, and client and system gestures remain open (#70, #252) (solium) [docs] <https://github.com/Lilium-Linux/solium/issues/70> <https://github.com/Lilium-Linux/solium/issues/252>
- 2026-11-07: On 2026-11-07 #1911 carried touchpad pinch only; #1913, stacked on it, adds shell bindings in sway bindgesture grammar through bindKeys and domicile.gestures with counted grabs, three-finger workspace swipes in Manganese, actions at gesture end, and a documented gap for windows with a shortcuts inhibitor [docs] <https://github.com/cprussin/domicile/pull/1911> <https://github.com/cprussin/domicile/pull/1913>
- 2026-11-07: On 2026-11-07 the maintainer stated in #1908 that a stack starting with touchpad pinch, swipe and hold would land, #1907 would be carried forward with physical-touchpad qualification. Review of #1907 had asked for a DRM and libinput physical-touchpad test [docs] <https://github.com/cprussin/domicile/issues/1908#issuecomment-6711470051> <https://github.com/cprussin/domicile/pull/1907>
- 2026-11-07: PR #1913 lists one known gap on 2026-11-07: a window holding a shortcuts inhibitor does not receive the shell gesture bindings [docs] <https://github.com/cprussin/domicile/pull/1913>
- 2026-11-07: In a VM test on 2026-11-07, a two-finger horizontal history swipe moved one entry at 1x page zoom and did nothing while the page was already zoomed in, repeatably [docs] <https://github.com/cprussin/domicile/pull/1907>

## Where it touches Domicile

- `engine:browser` holds the owner and routes every phase to that channel. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/desk_gestures.cc#L68) [source]
- `sdk` accumulates motion and resolves a finished gesture. Hold bindings complete only at an uncanceled end; that completion result carries no duration threshold. Backend recognition and begin-time ownership remain separate. [source] [Completion tracker](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L143) · [Hold completion](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L190)
- `manganese` supplies focus, workspace, scratchpad, overview and launcher bindings. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L111) [docs]
- Status at the pin: landed-with-gap. [#1913](https://github.com/cprussin/domicile/pull/1913) answers phase delivery and matching; [#1928](https://github.com/cprussin/domicile/pull/1928) answers unmatched native delivery. Shortcut inhibition suppresses shell gesture bindings, but does not yet deliver a grabbed gesture to the inhibited window. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L144) [docs]

## Lineage

- 2026-11-07: the reference survey compared begin-time ownership, native pass-through and continuous layout motion. S-d68dcb7e6552 · 2026-11-07 · Native gesture donor mechanisms [docs]
- 2026-11-09: the pinned gesture contract includes continuous phases, unbound forwarding and Manganese bindings. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md#L31) [docs]

## Sources

- S-34c3080edb40 · 2026-11-09 · Gesture daemon hold completion.

- S-f152f6bc05af · 2026-11-09 · Fusuma hold timer recognition.

- S-5599731427c4 · 2026-11-09 · Ash multi-finger gesture framing.

- S-0a6357b8c6cf · 2026-11-09 · KWin gesture candidate selection.

- S-e331d31605d8 · 2026-11-09 · Niri reserved swipe counts and native hold forwarding.

- S-774e974df80c · 2026-11-09 · Browser gesture channel ownership and unbound forwarding.
- S-a90d2490bf23 · 2026-11-09 · Native gestures, page defaults and shell binding contract.
- S-d2242e1a7d7f · 2026-11-09 · SDK gesture grammar and completion matching.
- S-d68dcb7e6552 · 2026-11-07 · Native gesture donor mechanisms.
