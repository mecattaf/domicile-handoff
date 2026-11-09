---
id: families/keyboard-and-text
title: keyboard-and-text features
kind: family
family: keyboard-and-text
layers: []
domicile_status: n/a
checked_at: "2026-11-09"
scroll_parity: n/a
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [bucket:domicile]
sources: [S-0f7230f8f6a6, S-5ff85c78489f]
---

# keyboard-and-text features

Typed chords, shortcut inhibition and text composition.

Statuses at the pin: 1 absent, 1 landed, 1 landed-with-gap.

- [Native chords and the typed key table](keybinding-model.md).
- [Shortcut inhibition for remote desktops and games](shortcuts-inhibit.md).
- [Text input, input methods and virtual keyboards](text-input-and-ime.md).
- [Other recorded contracts](facts.tsv): source rows without a separate feature page.

## Other recorded contracts

| Behavior | Contract at the pin | Evidence |
|---|---|---|
| GTK 4.20.0 client keyboard repeat | The per-seat client timer uses advertised repeat information when available, checks keymap repeatability, stops on focus loss and waits for display sync before timer delivery. This is donor source behavior. | [source](https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L1131) · [source](https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L1264) · [source](https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L1321) · [source](https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L1468) · [source](https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L1606) [source] |
| Qt Wayland 6.9.0 client keyboard repeat | The xkbcommon path starts a client timer for repeatable keys with positive rate; timer firings synthesize repeat release/press pairs, and focus loss stops the timer. This is donor source behavior. | [source](https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputdevice.cpp#L67) · [source](https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputdevice.cpp#L1347) · [source](https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputdevice.cpp#L1375) · [source](https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputdevice.cpp#L1403) [source] |

## Sources

Each feature links its pinned implementation and dated investigation sources.

- S-0f7230f8f6a6 · 2026-11-09 · GTK 4.20.0 client keyboard repeat source record.

- S-5ff85c78489f · 2026-11-09 · Qt Wayland 6.9.0 client keyboard repeat source record.
