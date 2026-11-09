---
id: "protocols/topics/input-method-and-virtual-keyboard"
title: "Text input, input methods and virtual keyboards"
kind: "topic"
layers: ["engine:browser", "compositor", "protocol", "sdk", "external"]
domicile_status: "absent"
domicile_refs: []
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: "baseline"
evidence_mix: "mostly-source"
boundary_note: true
bucket: "domicile"
tags: ["layer:protocol", "evidence:source"]
sources: [S-33085e8f5a27, S-3f524db52413, S-4aaee696f4de, S-4ddce4f277b2, S-534dcadd1369, S-9a9f91803d7e, S-bcc86c9a88ba]
family: protocols
---

# Text input, input methods and virtual keyboards

Text composition, an input-method service and a virtual keyboard are different protocol roles. Browser text editing and ordinary Wayland keyboard delivery do not demonstrate all three. [docs]

## Separate roles

| Protocol | Role | Pinned dependency availability | Domicile registration |
|---|---|---|---|
| `text-input-unstable-v3` | A client reports text-editing state and accepts composition/committed text | Smithay text-input-v3 manager | Absent |
| `input-method-unstable-v2` | An input-method service supplies composition to the active editor | Smithay input-method-v2 manager | Absent |
| `virtual-keyboard-unstable-v1` | A client supplies a keymap and keyboard events | Smithay virtual-keyboard handler | Absent |

Smithay provides these handlers at Domicile's dependency pin. The complete executable-compositor registration scan finds no corresponding global, state or delegate. [text input](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/text_input/mod.rs#L100), [input method](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/input_method/mod.rs#L154), [virtual keyboard](https://github.com/Smithay/smithay/blob/fb981077def1405300e69a5993e9789768e772aa/src/wayland/virtual_keyboard/mod.rs#L91), [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L817) [source]

## Existing input surfaces

The compositor creates an ordinary keyboard and delivers its compiled keymap to native clients. The SDK separately offers keymap selection and typed shell shortcut events. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L844), [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/keyboard.ts#L9) [source]

Those shell shortcut events and a browser guest accepting composed text do not establish a protocol path for composition into another native client. [inferred]

A client can also compose key sequences locally. Kitty 0.48.0 feeds its xkb compose state and emits composed UTF-8 independently of its optional Wayland text-input binding. An absent text-input global therefore does not establish failure of every compose sequence. [source] [Local compose state](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/xkb_glfw.c#L722) · [Key text production](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/xkb_glfw.c#L892)

Qt Wayland 6.9.0 also has a locale-based local compose path when built with xkbcommon. It feeds keysyms through a compose table and sends the result to the focus object separately from its text-input interface. [source] [Compose state](https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputcontext.cpp#L223) · [Compose delivery](https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputcontext.cpp#L253)


Related feature: [text input and input methods](../../06-features/keyboard-and-text/text-input-and-ime.md).

## Sources

- S-4ddce4f277b2 · 2026-11-09 · Qt Wayland local composition and text-input context.

- S-4aaee696f4de · 2026-07-18 · Client-local key composition.

- S-33085e8f5a27 · 2026-11-09 · Smithay input-method manager at the dependency pin.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-534dcadd1369 · 2026-11-09 · Smithay virtual-keyboard handler at the dependency pin.
- S-9a9f91803d7e · 2026-11-09 · Smithay text-input manager at the dependency pin.
- S-bcc86c9a88ba · 2026-11-09 · SDK shortcut, modifier and keymap interface.
