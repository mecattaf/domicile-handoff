---
id: "02-aura-ash-exo/input-gestures"
title: "Input and gestures"
kind: "collection"
domicile_status: "n/a"
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: "mixed"
bucket: "domicile"
tags: ["bucket:domicile", "evidence:source"]
sources: [S-07df81c87693, S-1eabe6bbdfa7, S-1f554a1713a5, S-2bb5f68bf0ba, S-60f59e73b96d, S-d9193550e1ab, S-24f0ff5009b7, S-a90d2490bf23]
---

# Input and gestures

Ash recognizers and cursor rules combine reusable behavior with ChromeOS policy. Linux engine event delivery and Ash controller availability are separate facts. [docs]

## What Chromium already has

The [row data](rows/aura-lift.tsv) holds 11 rows for this beat: ADAPT 2, FRESH 3, LIFT 6. These are historical source verdicts. [docs]

Donor availability in those rows: `chromeos-gated` 3, `in-engine-build` 8. Zero Ash references in a file do not bypass its target build gate. [docs]

- Exo still leaves its pointer swipe request unimplemented at this engine pin; its pinch delegate sends real begin, update and end events. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/zwp_pointer_gestures.cc#L30), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/wayland/zwp_pointer_gestures.cc#L52).
- The Ozone Wayland wrapper registers pinch and hold listeners; no swipe object or listener appears in the checked wrapper files. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_zwp_pointer_gestures.cc#L72), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/ozone/platform/wayland/host/wayland_zwp_pointer_gestures.cc#L87).
- Ash typing-hide ignores repeats, held mouse buttons and command chords, with additional virtual-keyboard and per-window exceptions. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ash/wm/cursor_manager_chromeos.cc#L68).
- Generic CursorManager implements typing-hide under a Windows build guard; the checked non-Windows branch returns false. [source] [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/cursor_manager.cc#L308), [Donor source](https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/ui/wm/core/cursor_manager.cc#L355).

## Domicile at the pin

- `engine, compositor and sdk`: The current gesture contract keeps phases, claims and native delivery distinct; shell bindings do not replace the native client protocol path. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/GESTURES.md).
- `config and engine:aura`: The public pointer-hiding change uses an Aura observer and cursor client; it is an in-review precedent for a rule port rather than evidence that Ash is compiled. [docs] [Pinned contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-CONFIG.md).
- Merged touchpad changes: [native pinch delivery #1911](https://github.com/cprussin/domicile/pull/1911), [per-window pinch ownership #1927](https://github.com/cprussin/domicile/pull/1927), [binding interface #1913](https://github.com/cprussin/domicile/pull/1913), [unclaimed swipe/hold delivery #1928](https://github.com/cprussin/domicile/pull/1928), and [multi-finger defaults #1929](https://github.com/cprussin/domicile/pull/1929). These changes distinguish matching, claiming and native delivery. [docs]
- Merged touchscreen and page changes: [native contacts #1924](https://github.com/cprussin/domicile/pull/1924), [edge matching #1925](https://github.com/cprussin/domicile/pull/1925), [spread actions #1941](https://github.com/cprussin/domicile/pull/1941), [browser pinch target #1944](https://github.com/cprussin/domicile/pull/1944), and scroll dynamics in [#1912](https://github.com/cprussin/domicile/pull/1912) and [#1943](https://github.com/cprussin/domicile/pull/1943). [docs]
- [Pointer hiding #1945](https://github.com/cprussin/domicile/pull/1945) remains `in-review` at the checked snapshot. Its Aura observer and cursor-client pattern is a separate availability precedent. [docs]

## Boundary

Matching a gesture, deciding who claims it, and delivering it to a Wayland client are different operations. Pointer visibility is separate from pointer focus. [inferred]

## Feature links

- [gestures/multi-finger-swipe-and-hold](../06-features/gestures/multi-finger-swipe-and-hold.md)
- [gestures/touchpad-pinch](../06-features/gestures/touchpad-pinch.md)
- [pointer/hide-while-typing](../06-features/pointer/hide-while-typing.md)

## Sources

- S-60f59e73b96d · 2026-07-18 · Chromium input and gestures source and equivalence inventory
- S-1f554a1713a5 · 2026-07-18 · Lift-map evaluation and corrections
- S-07df81c87693 · 2026-11-09 · Pinned donor interface inspection
- S-1eabe6bbdfa7 · 2026-11-09 · Pinned donor interface inspection
- S-2bb5f68bf0ba · 2026-11-09 · Pinned donor interface inspection
- S-d9193550e1ab · 2026-11-09 · Pinned donor interface inspection
- S-24f0ff5009b7 · 2026-11-09 pin check · Pinned shell configuration contract
- S-a90d2490bf23 · 2026-11-09 pin check · Pinned gesture matching and ownership contract
