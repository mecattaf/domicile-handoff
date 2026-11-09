---
id: GLOSSARY
title: Glossary
kind: collection
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
bucket: domicile
evidence_mix: mixed
tags: [bucket:domicile]
sources: []
---

# Glossary

Public names used across the library. Era and graveyard names link to their pages; each page states its dates from content, not from folders.

## Eras (01-chromium-lineage)

- [CADE: Ash and Aura on generic Linux DRM](01-chromium-lineage/cade.md): CADE explored Chromium as the desktop compositor: Ash supplied window-management machinery, Ozone supplied DRM presentation and Exo hosted Wayland applications.
- [COWL revived and the patch campaign](01-chromium-lineage/cowl-revived.md): The 2026-06-16 revival restored the browser-and-shell client boundary under niri.
- [COWL: Chromium as a layer-shell client](01-chromium-lineage/cowl.md): COWL was a Chromium Content API embedder and later a browser-plus-shell fork.
- [CUBS: one browser process, a native Views shell](01-chromium-lineage/cubs.md): The June 29 CUBS design argument described a tier-2 Chromium fork whose browser process also hosted native Views shell surfaces under niri.
- [Domicile: the shipped form recognized in November](01-chromium-lineage/domicile-recognition.md): A study written on 2026-11-04 recognized Domicile and Manganese as a shipped implementation of the recurring Chromium-desktop idea.
- [Headwaters: a web shell with a typed namespace](01-chromium-lineage/headwaters.md): The December 2025 web-shell ground truth describes a transparent web interface in a native host, separated from system services by a typed namespace.
- [The July 2026 native-shell floor](01-chromium-lineage/july-native-shell-floor.md): The July continuation rebuilt a native Views shell on Chromium 150 under niri.
- [A 2026-10-28 Chromium/Aura lineage report](01-chromium-lineage/october-aura-report.md): The October report reconstructed the compositor lineage while preserving the difference between original constraints, contemporary runtime reports and later interpretation.
- [quickshellX / webshell: web content under niri](01-chromium-lineage/quickshellx-webshell.md): quickshellX added QtWebEngine and QtWebChannel to a Quickshell host.
- [REVOLUTION: a purpose-built Chromium embedder](01-chromium-lineage/revolution.md): REVOLUTION reframed the shell host as a Chromium Content API embedder with direct Ozone work for Wayland surface roles.
- [views-shell and the cowl:// study](01-chromium-lineage/views-shell-and-cowl-scheme.md): The October study separated a browser-free Views shell client from a web system page.
- [Sway-next: the recorded compositor fork](04-sway-family/sway-next.md): Sway-next combined scroll's layout with sway-compatible names and commands, swayfx-style effects, and additional window and seat state.

## Closed investigations (05-graveyard)

- [The July Aura/Ash/Exo lift map](05-graveyard/aura-lift-july.md): A component survey classified reusable Chromium machinery, ChromeOS-coupled implementation and floor behavior requiring fresh expression.
- [CADE](05-graveyard/cade.md): CADE investigated Ash/Aura as the compositor, with DRM presentation and Exo application hosting on generic Linux.
- [COWL, its revival and patch campaign](05-graveyard/cowl.md): COWL was a native Chromium embedder and browser-plus-shell client under niri.
- [CUBS and the July native-shell floor](05-graveyard/cubs.md): The CUBS design argument combined browser services and native Views shell surfaces in a tier-2 Chromium fork under niri.
- [Electron and CEF as embedding donors](05-graveyard/electron-cef.md): The March study compared Electron, CEF offscreen rendering and a direct Chromium embedder for desktop shell surfaces.
- [GPUiX and the return to sway](05-graveyard/gpuix-aura-to-hyprland-and-return.md): The 2026-10-13..22 investigation explored a native React/Solid shell over a wlroots compositor.
- [Headwaters](05-graveyard/headwaters.md): A web shell in a native host exposed system data and operations through a typed namespace.
- [A multiplayer desktop on sway and VNC](05-graveyard/multiplayer-desktop-2022.md): A 2022-23 desktop combined sway with VNC access.
- [The October Otto native-shell investigation](05-graveyard/otto-surge.md): A 2026-10-23..28 investigation examined Otto as a patched compositor base and as the host for a native shell.
- [Otto on a virtual terminal](05-graveyard/otto-trial.md): A 2026-10-06 study explored Otto as a Skia-based desktop on a virtual terminal, alongside a retained scroll session.
- [quickshellX / webshell](05-graveyard/quickshellx-webshell.md): A QtWebEngine host rendered a web shell under niri.
- [The sway-next compositor study](05-graveyard/sway-next.md): The 2026-10-22..23 sway-next study explored a compositor fork that kept sway-compatible commands while retaining scroll layout, overview, jump and Lua semantics.
- [views-shell and the cowl:// study](05-graveyard/views-shell-and-cowl-scheme.md): The October study separated a browser-free Views client from a web system page and its privileged browser seam.
- [Window-manager scoping in October 2026](05-graveyard/wm-scoping-2026-10.md): The 2026-10-06 scoping compared scroll, niri and a standalone Chromium Views shell.
- [A workspace sidebar beside sway](05-graveyard/workspace-sidebar-spike.md): A 2026-10-26..11-03 spike kept applications in Chrome while a separate native panel sat beside sway.

## Terms

- **Domicile**: the Chromium-based Wayland compositor and desktop at the pinned revision; **Manganese** is its reference shell; **the engine** is its Chromium fork.
- **Donor**: a project whose mechanism or semantics a page describes for comparison: sway, scroll, niri, Hyprland, KDE, mutter, ChromeOS ash and exo, Aura, Views, macOS, Smithay compositors, lisgd, fusuma, libinput-gestures, miracle-wm, otto, kitty.
- **scroll**: the sway fork with a scrolling layout (dawsers/scroll); **scroll parity** means matching its semantics, the stated ideal of this library.
- **swayfx, sway-next, any1**: sway derivatives and branches carrying effects, seats and layout additions; their additions are tagged `scroll-parity: beyond`.
- **Aura / Ash / Exo**: Chromium's windowing layer, ChromeOS's window manager and ChromeOS's Wayland server; pages record which parts the Linux engine build compiles and which are ChromeOS-gated.
- **Views**: Chromium's native UI toolkit; concepts from it are cited for behavior only.
- **Native-shell floor**: the July 2026 set of behaviors a native shell had to meet before it counted as usable; cited as history.
- **Open boundary**: a one-sentence marker that a topic (programmatic desktop control) is deliberately outside this library.
- **Living list**: reproducible symptoms observed while using Domicile, each tied to a feature page and its upstream state at the pin.
- **Pin**: the single Domicile revision every status is checked against; see `SEAL.md` and the pins table.
- **S-id**: an opaque identifier of a source document in the notes (`S-` plus twelve hex characters); resolvable on request, never a file name.
