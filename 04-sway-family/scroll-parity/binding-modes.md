---
id: sway-family/scroll-parity/binding-modes
title: Binding modes and shortcuts
kind: family
family: bindings-and-modes
layers: [sdk, manganese, compositor, protocol, engine:browser]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-0d9c2ee30def, S-1b032775d560, S-204a969601d0, S-23c521755f22, S-3606bb231e4a, S-3a84a5d438a4, S-4a2b900f6a8d, S-7f4a19466a2d, S-8c438f028a0f, S-9fe5a8a3f191, S-a70560fdc087, S-d2242e1a7d7f, S-d5b15decb594, S-fbe8a9d8422d]
---

# Binding modes and shortcuts

107 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 74 same; 26 scroll-changed; 7 scroll-only; 0 sway-only.
- Domicile coverage: 4 landed; 12 partial; 90 absent; 0 unverified; 1 accounting aliases/exclusions.
- Mechanism layers: compositor 1; engine:browser 6; manganese 106; protocol 1; sdk 106.
- Feature: [bindings-and-modes/binding-modes](../../06-features/bindings-and-modes/binding-modes.md).

## Donor semantics

A binding mode switches the active command table. Donor contracts also specify keysym versus keycode matching, input-device and layout-group selection, key release, repeated presses, lock/inhibition behavior and mouse target regions. Those flags are individually counted instead of being collapsed into the existence of key bindings. [source] [Scroll bindings](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1153) · [Modes](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1685).

Scroll changes release timing: its documented release action waits until the main key and modifiers are all released. Matching a binding name in another system does not establish that edge behavior. [source] [Release semantics](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1228).

Manganese uses SDK binding tables and a named mode setter. Its action vocabulary and parser are narrower than the donor configuration language. Current source stores one mode in the shared desktop state and passes it into the binding hook, which applies it through the SDK setter. [source] [Bindings](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L84) · [Command table](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35).

The shared-state field and hook establish desktop-wide mode scope at the pin. [source] [Shared mode](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/window-state.ts#L188) · [Mode synchronization](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/useKeybindings.ts#L68).
The shortcut registration and binding hook expose no per-binding lock-safe eligibility option. Generic callback delivery while locked is not established by that declaration shape; lock-screen input and keymap calls allowed while locked remain separate contracts. [source] [Registration](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L749) · [Binding hook](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/bind-keys.ts#L59).

The donor binding selector prefers a matching device, then an explicit XKB group, then the applicable lock and inhibition state. Domicile's complete chord parser accepts recognized modifiers and one keysym; its SDK dispatches the resulting chord in the current mode. Re-resolving a keysym when the layout changes is distinct from exposing per-device or per-group override rules. [source] [Donor precedence](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/keyboard.c#L200) · [Chord parser](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_chord.cc#L33) · [SDK dispatch](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/bind-keys.ts#L126).

The SDK reserves the union of chords from every configured mode while dispatching actions only from the active table. Mode changes therefore select actions without rebuilding that union of native grabs. The source separately releases successful grabs on unbind and stops an opening command subscription even if it becomes ready after teardown. These lifetimes do not establish inactive-mode pass-through or selective lock-safe delivery. [source] [Union of grabs](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/bind-keys.ts#L69) · [Active-table dispatch](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/bind-keys.ts#L126) · [Teardown](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/bind-keys.ts#L157).

No-repeat is documented as the current default. The page's matched-key handler prevents the ordinary page action and discards repeated keydown events before dispatching a shortcut. This default is separate from an exposed per-binding repeat-policy flag. The donor no-repeat flag remains shared between Sway and Scroll: its repeat timer is omitted when the flag is set. [source] [Current repeat guard](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1008) · [Donor timer guard](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/keyboard.c#L604) · [Sway flag contract](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway.5.scd#L468).

The release implementation tests every still-recorded key, which can include keys beyond the captured chord. Matching a release candidate compacts already-released history; a pressed binding clears both history and the pending release candidate. This bounds the documented main-key/modifier rule with the current state machine. [source] [Release history](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/keyboard.c#L433) · [Release dispatch](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/keyboard.c#L541) · [Press cancellation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/keyboard.c#L617).

SDK gesture completion prefers explicit finger-count or edge selectors, then the binding requesting more directions. Swipe completion adds a diagonal direction when minor-axis travel reaches half the dominant travel. Hold completion requires an uncanceled end but carries no duration threshold. These rules do not establish pinch-translation matching or backend hold-recognition timing. [source] [Completion selection](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L203) · [Completion selection](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L225) · [Hold completion](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L143) · [Hold completion](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L167) · [Hold completion](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/gesture-binding.ts#L190).

The renderer updates each window’s inhibition flag and checks the keyboard-focused one for recognized page chords and gesture dispatch. Unmatched or inhibited page chords return before consumption. Matched keydown events are consumed, with repeats stopping before shortcut dispatch. The separate native ShortcutPressed callback calls FireShortcut directly and supplies no local repeat/inhibition check; upstream native producer qualification remains separate. [source] [State and focused lookup](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L987) · [Page dispatch](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1008) · [Native callback and gestures](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1639) · [State update](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L2301).

Document-to-native key forwarding is a separate producer path. It consumes keydown and drops browser repeats before emitting a pressed Key request; page-taken presses are recorded for matching release suppression. Native keyup carries the recorded press-owner identifier when present, otherwise the current target. Blur, page hiding and browser-guest focus request releases for held entries. These source-level guards and request identifiers remain separate from repeat-rate advertisement and final seat/client delivery. [source] [Native producer repeat guard](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1302) · [Forwarded press](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1323) · [Release owner](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1341) · [Focus/visibility cleanup](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1369).

## At the pin

Not addressed at the pin: the exact donor binding flag, explicit unbind operation or markup option is not exposed by Manganese command/configuration tables. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/command.ts#L35) · [Current contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L38).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-0d9c2ee30def · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-1b032775d560 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-3606bb231e4a · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-3a84a5d438a4 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
- S-7f4a19466a2d · 2026-11-05 · Scroll reference at the donor revision.
- S-8c438f028a0f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-9fe5a8a3f191 · 2026-09-21 · sway reference at the donor revision.
- S-a70560fdc087 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-d2242e1a7d7f · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-fbe8a9d8422d · 2026-11-09 · Domicile shell and typed-state reference at the pin.
