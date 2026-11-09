---
id: "protocols/topics/seats-and-multiseat"
title: "Seats and independent input participants"
kind: "topic"
layers: ["engine:input", "engine:drm", "engine:browser", "compositor", "protocol", "session", "external"]
domicile_status: "n/a"
domicile_refs: []
checked_at: "2026-11-09"
scroll_parity: "baseline"
evidence_mix: "mostly-source"
boundary_note: true
bucket: "domicile"
tags: ["layer:protocol", "evidence:source"]
sources: [S-0e23913ab733, S-0f7230f8f6a6, S-1951a846e891, S-220a8533ce51, S-25309f3f163f, S-2871d1466900, S-3226f0a93193, S-3a703c6ea2fd, S-3f524db52413, S-4ddce4f277b2, S-70bbd2a8c43a, S-76824356a8c6, S-834d9f2aa271, S-9298e590438f, S-9b6eacc885db, S-dd20e74809b3, S-ee2dbf3a278d, S-f152f60cb06a]
family: protocols
---

# Seats and independent input participants

The word seat names three separate mechanisms: permission to use devices, a group of logical input devices and focus, or a protocol that creates a temporary logical seat. One does not imply the others. [docs]

## Three meanings

| Meaning | Mechanism | Domicile at the pin |
|---|---|---|
| Device access | logind or libseat/seatd brokers DRM and input descriptors and session activation | logind owns device access and DRM master; the engine uses Chromium D-Bus, with no libseat |
| Logical Wayland seat | A `wl_seat` groups keyboard, pointer and touch capabilities, with focus and selections | One seat is created at startup |
| Temporary logical seat | `ext-transient-seat-v1` creates a seat with a client-bound lifetime | No manager is registered |

The pinned tty contract documents the first row. The seat construction and complete executable-module registration inspection establish the other two. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/TTY-SESSION.md#L7), [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L834) [docs] [source]

## What independent focus does and does not establish

- A second person, kiosk, testing client or independent remote participant can benefit from a separate pointer and keyboard focus. A logical seat is an input organization mechanism. It is not, by itself, a security boundary. [docs]
- Visible workspace selection, binding scope, launch focus, clipboard selection, popup serials and client seat selection can remain shared even when pointers are separate. S-3226f0a93193 · 2026-10-22 · Logical-seat isolation and client compatibility study [docs]
- A virtual keyboard or pointer targeting the existing seat does not create another participant. Domicile's single seat and engine desktop remain the relevant boundary at the pin. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L834) [inferred]
- An application can ignore additional seats even when the server advertises them. Kitty 0.48.0 stores its first bound seat and ignores later seat globals. [Kitty seat binding](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_init.c#L604) [source]

Alacritty 0.17.0 locks winit 0.30.13, whose Wayland backend keeps input resources per seat. Its window focus combines the focused seats, and keyboard events in this path use a shared Wayland device identifier. Per-seat resources therefore establish neither an independent application caret nor a per-seat application input identity. [source] [Locked client dependency](https://github.com/alacritty/alacritty/blob/v0.17.0/Cargo.lock#L2637) · [Seat resources](https://github.com/rust-windowing/winit/blob/v0.30.13/src/platform_impl/linux/wayland/seat/mod.rs#L73) · [Window focus](https://github.com/rust-windowing/winit/blob/v0.30.13/src/platform_impl/linux/wayland/seat/keyboard/mod.rs#L68) · [Keyboard device identity](https://github.com/rust-windowing/winit/blob/v0.30.13/src/platform_impl/linux/wayland/seat/keyboard/mod.rs#L374)

EIS keeps pointer coordinates and held buttons per client. Its direct translation route sends movement and button events as ordinary compositor requests through the existing lock gate. That bookkeeping is distinct from creating another `wl_seat` with independent focus. [source] [Per-client translation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/translation.rs#L103) · [Button requests](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/translation.rs#L119)

GTK 4.20.0 binds seat globals and creates capability-specific pointer and keyboard resources per seat. It also creates per-seat data objects, while display-wide clipboard references are initialized only when unset; the generic default-seat accessor takes the first seat in the current list. [source] [Seat binding](https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkdisplay-wayland.c#L328) · [Seat capabilities](https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L2413) · [Clipboard objects](https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/wayland/gdkseat-wayland.c#L4495) · [Default seat](https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gdk/gdkdisplay.c#L201)

Qt Wayland 6.9.0 stores an input-device object for every seat global. Its default-device accessor selects the first entry, while keyboard-availability checks inspect every entry. These resource and default-selection paths do not establish separate application carets. [source] [Seat collection](https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylanddisplay.cpp#L659) · [Default and availability checks](https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylanddisplay.cpp#L1050)

The GTK 4.20.0 Wayland IM context creates text-input-v3 for the display’s default seat and stores one current editing context in display state. Qt Wayland 6.9.0 likewise selects its default input device for text input and directs updates through the application focus object and window. These source selectors further limit what multiple seat resources alone establish. [source] [GTK default seat](https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gtk/gtkimcontextwayland.c#L714) · [GTK display state](https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gtk/gtkimcontextwayland.c#L765) · [GTK current context](https://github.com/GNOME/gtk/blob/71926a295583bc44c1c52c4d7b74f375381d68b2/gtk/gtkimcontextwayland.c#L789) · [Qt default device](https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputcontext.cpp#L77) · [Qt focus object](https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputcontext.cpp#L177) · [Qt focus window](https://github.com/qt/qtwayland/blob/v6.9.0/src/client/qwaylandinputcontext.cpp#L218)

## Evidence limits

The historical donor study describes sway 1.12, wlroots 0.20.2 and wayvnc 0.10.1. It records independent focus for multiple connections, along with runtime device reassignment and shared-output workspace effects. Those results are dated donor observations, labeled `[docs]` here. They are not measurements of Domicile at this pin. S-3226f0a93193 · 2026-10-22 · Logical-seat isolation and client compatibility study [docs]

The same study records secondary-seat typing in foot and alacritty and a first-seat-only kitty control. It leaves GTK/Qt behavior, actual drawn cursors per seat and Chrome runtime behavior unproven. S-2871d1466900 · 2026-10-22 · Multi-seat client input study [docs]

The pinned tty status also leaves hardware input recovery after a full console round trip unverified. This device-lifetime limit belongs to the existing logind route, independently of multi-seat. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/A-DESKTOP-ON-A-TTY.md#L33) [docs]

## Lineage

- 2026-06-06: the session study separates device brokerage, Ozone acquisition and Wayland delivery. S-9298e590438f · 2026-06-06 · Linux session and input acquisition study [docs]
- 2026-10-06: the input comparison records scroll's multiple-seat surface and niri's single-seat construction. S-76824356a8c6 · 2026-10-06 · Input and logical-seat source comparison [docs]
- 2026-10-22: a bounded donor study records focus isolation, command-context coupling and application seat selection. S-3226f0a93193 · 2026-10-22 · Logical-seat isolation and client compatibility study [docs]


Related features: [independent logical seats](../../06-features/seats/independent-seat.md), [device access and tty session](../../06-features/seats/libseat-and-session.md).

## Sources

- S-4ddce4f277b2 · 2026-11-09 · Qt Wayland default text-input device and application focus.

- S-25309f3f163f · 2026-11-09 · GTK Wayland display-scoped text-editing context.

- S-834d9f2aa271 · 2026-11-09 · Qt Wayland input-device collection and defaults.

- S-3a703c6ea2fd · 2026-11-09 · GTK default-seat selection.

- S-0f7230f8f6a6 · 2026-11-09 · GTK per-seat resources and clipboard objects.

- S-0e23913ab733 · 2026-11-09 · GTK Wayland seat binding.

- S-220a8533ce51 · 2026-11-09 · Remote-input client state and request translation.

- S-dd20e74809b3 · 2026-11-09 · Winit focus aggregation and keyboard event identity.

- S-70bbd2a8c43a · 2026-11-09 · Winit per-seat input resources.

- S-ee2dbf3a278d · 2026-11-09 · Alacritty release dependency lock.

- S-1951a846e891 · 2026-11-09 · Direct-session architecture and native device ownership.
- S-2871d1466900 · 2026-10-22 · Multi-seat client input study.
- S-3226f0a93193 · 2026-10-22 · Logical-seat isolation and client compatibility study.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-76824356a8c6 · 2026-10-06 · Input and logical-seat source comparison.
- S-9298e590438f · 2026-06-06 · Linux session and input acquisition study.
- S-9b6eacc885db · 2026-07-18 · Kitty seat and pointer-gesture registry binding.
- S-f152f60cb06a · 2026-11-09 · Logind card, input and console lifetime contract.
