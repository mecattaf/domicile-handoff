---
id: pointer/hide-while-typing
title: Hide the pointer while typing and after inactivity
kind: feature
family: pointer
layers: [engine:aura, config]
domicile_status: in-review
domicile_refs: [cprussin/domicile#1932, cprussin/domicile#1945]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, ash, macos, hyprland, aura]
rows: []
evidence_mix: mostly-source
donor_availability: rule-portable
boundary_note: false
bucket: domicile
tags: [family:pointer, status:in-review, evidence:source, scroll-parity:baseline]
sources: [S-07df81c87693, S-0c2272ae6a84, S-1eabe6bbdfa7, S-3533e3f0274a, S-562a911b63fa, S-817625bbdcf4, S-866b9f3f829d, S-9999b24aea51, S-9ca9233a2f5e, S-cb7754dcb949, S-d34e89f64bf1, S-ecde7a8a4e5b, S-eec824252127]
---
# Hide the pointer while typing and after inactivity

The reviewed change hides the pointer image during typing or after an inactivity timeout, while keeping pointer focus and hover intact. [#1945](https://github.com/cprussin/domicile/pull/1945) [docs]

## Behavior

- A pointer move, button, wheel, scroll or pinch reveals it. A synthetic move, touch event or another typed key does not reveal it. [#1945](https://github.com/cprussin/domicile/pull/1945) [docs]
- A held button pauses the inactivity timer, so a drag does not hide midway. Typing suppression follows the native event filter rather than every key event indiscriminately. [#1945](https://github.com/cprussin/domicile/pull/1945) [docs]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | `seat hide_cursor` timeout and typing policy | Its hide behavior can clear pointer focus and close a hover tooltip | [public issue](https://github.com/swaywm/sway/issues/6200) |
| scroll | Carries sway seat cursor policy | Timeout is independent of a window gesture binding | [pinned manual](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-input.5.scd) |
| Ash @ engine pin | Filters repeats, held mouse buttons and command chords before hiding | Additional virtual-keyboard and per-window exceptions depend on ChromeOS state | [Pinned filter](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/cursor_manager_chromeos.cc#L68) · [Portable rule discussion](https://github.com/cprussin/domicile/pull/1945) |
| macOS / AppKit, documentation checked 2026-11-09 | `setHiddenUntilMouseMoves` has its own visibility state | Mouse movement or an explicit reset clears it; ordinary `unhide` does not | [API contract](https://developer.apple.com/documentation/appkit/nscursor/sethiddenuntilmousemoves%28_%3A%29) [docs] |
| Hyprland @ source pin | Inactivity is configured in seconds; zero disables the timeout | Key, touch and tablet hiding use separate switches | [Cursor configuration](https://github.com/hyprwm/Hyprland/blob/ab6141897d57e609e24c571a762c30c963e0885a/src/config/values/ConfigValues.cpp#L703) |
| Aura @ engine pin | The root cursor client decides whether key events hide its image | The generic Linux cursor manager returns false for that typing policy; Aura availability alone supplies no desktop typing-hide rule | [Event filter](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/compound_event_filter.cc#L207) · [Generic default](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/cursor_manager.cc#L308) |

## What the notes found

- The recorded issue separates cursor-image visibility from synthetic pointer departure. Keeping hover permits a tooltip to stay while the image is hidden. [docs]
- 2026-11-09: CompoundEventFilter delegates typing-hide policy to the root cursor client; synthesized events cannot change visibility. Generic CursorManager returns false outside Windows, so Linux Aura presence does not supply that typing policy (aura@domicile-engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/compound_event_filter.cc#L164> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/compound_event_filter.cc#L207> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/cursor_manager.cc#L308>
- 2026-11-09: Mouse-enter, movement, presses and wheel events can restore cursor visibility and mouse events. Touch-origin and synthesized events are excluded from those visibility changes (aura@domicile-engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/compound_event_filter.cc#L164> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/compound_event_filter.cc#L181> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/compound_event_filter.cc#L217>
- 2026-11-09: Generic CursorManager implements typing-hide under a Windows build guard; the checked non-Windows branch returns false (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/cursor_manager.cc#L308> <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/cursor_manager.cc#L355>
- 2026-11-09: Ash typing-hide ignores repeats, held mouse buttons and command chords, with additional virtual-keyboard and per-window exceptions (Chromium@159.0.8137.0) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/cursor_manager_chromeos.cc#L68>
- 2026-11-08: Sway offers seat hide_cursor with a timeout and a when-typing option; the issue notes sway hiding sends pointer-leave to the window, tracked in sway issues 6200 and 7383, while Chromium's HideCursor changes only the image (sway@1652c54) [docs] <https://github.com/swaywm/sway/issues/6200> <https://github.com/swaywm/sway/issues/7383> <https://github.com/cprussin/domicile/issues/1932>
- 2026-10-16: Publicly linked configuration snapshots place inactivity hiding on the seat, independently of pointer-follow focus policy and cursor appearance (public sway and scroll configurations) [docs] <https://github.com/cprussin/domicile/issues/1932>
- 2026-11-08: Issue #1932 (2026-11-08) asks for pointer hiding after inactivity and while typing; it places the work in the engine through aura's HideCursor and ShowCursor, with two compositor settings, cursor_hide_after_ms and cursor_hide_when_typing [docs] <https://github.com/cprussin/domicile/issues/1932>
- 2026-11-08: Issue #1932 cites sway seat hide_cursor timeout and hide_cursor when-typing, Hyprland cursor:inactive_timeout and cursor:hide_on_key_press, and niri hide-after-inactive-ms and hide-when-typing, each confirmed against source at fixed commits; Domicile main had no such option (sway@1652c54b73f67df17b7b4ab0b0f7048204aa8104) [docs] <https://github.com/cprussin/domicile/issues/1932>
- 2026-11-06: For native app windows the CSS cursor contract could hide the cursor on timeout or typing; client-provided cursor images, cursor magnification and embedded-webview behavior remained separate cases [docs]

## Where it touches Domicile

- `engine:aura` is the reviewed change location: an event observer drives the existing cursor visibility client. Donor availability is rule-portable. [#1945](https://github.com/cprussin/domicile/pull/1945) [docs]
- `config` in the reviewed change adds a timeout and a typing toggle; [#1932](https://github.com/cprussin/domicile/issues/1932) records the behavior and [#1945](https://github.com/cprussin/domicile/pull/1945) is its implementation review. [docs]
- Status at the pin: in-review. The public pull request is open against the pinned main revision. The behavior is not marked answered upstream. [#1945](https://github.com/cprussin/domicile/pull/1945) [docs]

## Lineage

- 2026-11-08: the recorded issue compared sway focus-leave behavior with Aura image visibility. [public issue](https://github.com/cprussin/domicile/issues/1932) [docs]
- 2026-11-09: [#1945](https://github.com/cprussin/domicile/pull/1945) remains open in the reviewed public state. [docs]

## Sources

- S-1eabe6bbdfa7 · 2026-11-09 · Ash typing-hide event filter and state exceptions.

- S-07df81c87693 · 2026-11-09 · Generic cursor-manager typing policy.

- S-9999b24aea51 · 2026-11-09 · Aura cursor visibility event routing.

- S-562a911b63fa · 2026-11-09 · Hyprland pointer visibility settings.

- S-cb7754dcb949 · 2026-11-09 · AppKit cursor visibility API contract.

- S-866b9f3f829d · 2026-11-08 · Public input and desktop compatibility history.
- S-eec824252127 · 2026-11-09 · Open pointer visibility implementation review.
