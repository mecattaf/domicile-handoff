---
id: keyboard-and-text/text-input-and-ime
title: Text input, input methods and virtual keyboards
kind: feature
family: keyboard-and-text
layers: [engine:browser, compositor, protocol, sdk, external]
domicile_status: absent
domicile_refs: []
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, smithay, otto]
rows: [input-method-v2, text-input-v1, text-input-v3, virtual-keyboard-v1]
evidence_mix: mostly-source
boundary_note: true
bucket: domicile
tags: [family:keyboard-and-text, status:absent, evidence:source, scroll-parity:baseline]
sources: [S-2124dd3c5784, S-2c3df898868f, S-33085e8f5a27, S-34997c703058, S-3f524db52413, S-43af2b3af183, S-534dcadd1369, S-5f357d1e3c92, S-725f15f061b0, S-827745f5e676, S-9a9f91803d7e, S-9b4baea3b316, S-e3d18f39c7a3, S-e7392f226f69, S-edb1143ace97, S-ef86748c2a79]
---
# Text input, input methods and virtual keyboards

Text-input, input-method and virtual-keyboard protocols cover different parts of text entry: application editing state, an input-method service and injected key events. Advertising a keyboard alone supplies none of those extra interfaces. [docs]

## Behavior

- A text-input client exchanges composition and surrounding-text state with the compositor. An input-method service supplies preedit and committed text. A virtual keyboard provides a keymap and keyboard events. [docs]
- Domicile native clients receive the seat keyboard and keymap. Browser editing inside Chromium does not establish an input-method route to another Wayland client. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L844) [inferred]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | One input-method connection per seat; another receives unavailable | Composition is relayed only to a focused, enabled text input | [Seat relay](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/text_input.c#L574) · [Application state](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/text_input.c#L259) |
| scroll | Retains the per-seat relay and focus/enable gate | Preedit, committed text and deletions precede done; focus changes leave and disable the previous editor | [Seat relay](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/text_input.c#L577) · [Text delivery](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/text_input.c#L37) · [Focus transition](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/text_input.c#L660) |
| Smithay | Provides independent text-input, input-method and virtual-keyboard modules | An available handler is not an advertised compositor global | [pinned modules](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/mod.rs#L48) |
| Otto @ 1.8.0 | Creates text-input-v3 for its first keyboard seat and reports the caret rectangle | The inspected reporter does not enable text input; the event callback handles neither preedit nor committed text | [Seat setup](https://github.com/nongio/otto/blob/v1.8.0/components/otto-kit/src/app_runner/mod.rs#L1073) · [Event callback](https://github.com/nongio/otto/blob/v1.8.0/components/otto-kit/src/app_runner/mod.rs#L2123) · [Caret reporter](https://github.com/nongio/otto/blob/v1.8.0/components/otto-kit/src/app_runner/context.rs#L2001) |

## What the notes found

- The protocol map keeps manager availability distinct from compositor registration. A browser or shell text box accepting Unicode does not demonstrate native text-input composition. [inferred]
- 2026-10-02: A 2026-10-02 Views-shell account traced generated text from a temporary virtual keyboard through wl_keyboard.enter, widget activation and FocusManager. It did not qualify physical keyboard-seat behavior (Views shell@154.0.8037.92) [docs]
- 2026-10-23: The Otto 1.8 runtime bound the text-input protocol but left it disabled and discarded composition callbacks; binding presence alone did not establish usable composition (otto@v1.8.0) [docs]

## Where it touches Domicile

- `external` Smithay provides a text-input-v3 manager, input-method-v2 manager and virtual-keyboard-v1 handler at the pinned dependency. [text input](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/text_input/mod.rs#L100), [input method](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/input_method/mod.rs#L154), [virtual keyboard](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/virtual_keyboard/mod.rs#L91) [source]
- Status at the pin: absent. The complete executable-compositor registration search finds no state, delegate or global for those three managers. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L817) [source]

## Lineage

The protocol history is indexed by collection.


## Sources

- S-ef86748c2a79 · 2026-11-09 · Smithay Wayland module and feature gates.

- S-2c3df898868f · 2026-11-09 · Otto 1.8.0 text-input setup, callback and caret reporting.

- S-725f15f061b0 · 2026-11-09 · Otto 1.8.0 text-input setup, callback and caret reporting.

- S-e3d18f39c7a3 · 2026-11-09 · Otto 1.8.0 text-input setup, callback and caret reporting.

- S-9b4baea3b316 · 2026-11-09 · Otto 1.8.0 text-input setup, callback and caret reporting.

- S-5f357d1e3c92 · 2026-11-09 · Scroll text-input focus and per-seat input-method relay.

- S-edb1143ace97 · 2026-11-09 · Sway text-input focus and per-seat input-method relay.

- S-43af2b3af183 · 2026-11-09 · Pinned donor implementation locator.

- S-e7392f226f69 · 2026-11-09 · Pinned donor implementation locator.

- S-33085e8f5a27 · 2026-11-09 · Smithay input-method manager at the dependency pin.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-534dcadd1369 · 2026-11-09 · Smithay virtual-keyboard handler at the dependency pin.
- S-9a9f91803d7e · 2026-11-09 · Smithay text-input manager at the dependency pin.
