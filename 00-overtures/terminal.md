---
id: overtures/terminal
title: The terminal as a Wayland client
kind: overture
family: terminal
layers: [engine:input, compositor, protocol, manganese, external]
domicile_status: n/a
domicile_refs: ["cprussin/domicile#1881", "cprussin/domicile#1882"]
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [kitty]
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:terminal, layer:protocol, donor:kitty, evidence:source]
sources: [S-0062bc389108, S-18c9c5046401, S-23c521755f22, S-27501c9817a7, S-3f524db52413, S-425cf9befbb2, S-42ecb235c7f2, S-5083a52eb56d, S-623d8e5fefeb, S-63da10a11f44, S-6ae15bd6541f, S-6ee2a1600602, S-882cc3aa0f38, S-997e756f69fc, S-9b6eacc885db, S-a886bb84a90d, S-ab93695e401f, S-c3ab182267b1, S-fd20eb1c6dd6]
---

# The terminal as a Wayland client

Kitty is an ordinary native Wayland client. It binds the compositor, shell and seat globals through its own Wayland backend; terminal rendering and child input and output remain client-side. [source] [Pinned client registry](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_init.c#L580), [client rendering model](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/docs/performance.rst#L4). The [protocol integration page](../06-features/terminal/kitty-wayland-protocols.md) separates that client contract from desktop policy. [docs]

## What changes around the window

- **Rows and title bars:** a terminal grid fits inside its allocated content box. Bars, gaps and window decoration reduce that box; fullscreen changes it again. Historical grid counts depended on a particular font and display geometry. See [window groups and title bars](../06-features/browser-windows/tab-groups-and-strips.md) and [scrolling layout](../06-features/layout/scrolling-layout.md). [docs]
- **Focus:** ordinary native windows follow pointer focus without being raised on hover. Keyboard delivery follows the focused window; Kitty's internal splits keep their own terminal focus. See [focus and urgency](../06-features/windows/focus-and-urgency.md). [docs] [Pinned focus policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L64)
- **Repeat:** the pinned Domicile keyboard setup uses 25 Hz with a 200 ms delay. Pinned sway defaults to 25 Hz and 600 ms unless configuration overrides it. Repeat onset is separate from initial keypress latency. See [client keyboard repeat](../06-features/terminal/kitty-wayland-protocols.md). [source] [Domicile setup](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L844) · [sway defaults](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/keyboard.c#L1087)
- **New native windows:** ordinary Kitty toplevels enter a tab group in Manganese. Floating rules and parented or fixed-size windows use their own placement path. Kitty's internal splits remain inside the terminal. See [window groups](../06-features/browser-windows/tab-groups-and-strips.md). [source] [Window insertion](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L6) · [Placement exceptions](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L204)
- **Middle click:** Kitty defaults to PRIMARY paste on an unmodified middle-button release when its child has not grabbed mouse input. Shift+middle release also covers the grabbed case. Domicile supplies the selection protocol; the binding belongs to Kitty. See [selection and drag-and-drop](../06-features/clipboard-dnd/data-control-and-primary-selection.md). [source] [Client defaults](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/kitty/options/definition.py#L993)
- **Terminal launch entries:** the pinned launcher and MIME handler both skip desktop entries marked `Terminal=true`; they have no configured terminal wrapper. A direct Kitty executable remains a normal launch command. See [Kitty integration](../06-features/terminal/kitty-wayland-protocols.md). [source] [Launcher filter](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-apps/src/desktop-entry.ts#L36), [MIME-handler filter](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/mime_apps.rs#L96).

## SSH carries bytes to a remote host

SSH connects the local terminal to a remote process. Terminal rendering stays local; network transport, the remote shell and application scheduling add separate timing boundaries. An SSH connection alone does not retain a remote PTY after that connection ends. Process lifetime, reconstruction of a retained terminal screen, and the lifetime of an editor's unsaved buffers are separate contracts. See [SSH and latency](../06-features/terminal/kitty-over-ssh-and-latency.md). [docs]

The recorded software echo figures exclude physical input, Kitty GUI presentation, compositor scheduling and display scanout. Local PTY observations and network byte-arrival observations use different boundaries; their numbers cannot be added into a measured input-to-pixel total. [docs]

| Documentary software cohort | Idle median | Busy median | Scope |
|---|---:|---:|---|
| Original local direct PTY | 0.176 ms | 0.042 ms | Local byte arrival, native terminal parser observer |
| Repeated local direct PTY | 0.180 ms | 0.047 ms | Same local study, repeated run |
| Ordinary SSH, interactive timing protection active | 13.107 ms | 10.373 ms | Ongoing network echo |
| Ordinary SSH, timing protection explicitly disabled | 3.067 ms | 2.163 ms | Separate transport policy, same network study |

These are recorded study results [docs], not measurements of Domicile's presentation path. An earlier 2026-11-07 echo study recorded a 0.110 ms direct-PTY idle median with another observer; it is a separate cohort. Quiet first keys and ongoing bursts are also distinct. Perceived responsiveness has no matched physical comparison in these sources. [docs]

A periodically refreshed intermediate renderer adds another schedule. If it forwards only completed redraws, streaming output reaches the final terminal at those redraw boundaries. This is a scheduling inference, separate from the recorded byte-arrival results. [inferred]

## A browser terminal has another boundary

Wraithterm's native profile combines a WebAssembly terminal state engine, a worker and a WebGPU renderer. [source] [Release architecture](https://github.com/betterclever/wraithterm/blob/v0.5.0/README.md#L96) The pinned shell policy permits WebAssembly and same-origin scripts; blob and data scripts and workers remain refused. Wraithterm 0.5.0 constructs its module worker with a module-relative URL. [source] [Shell policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-PACKAGING.md#L199) · [Worker construction](https://github.com/betterclever/wraithterm/blob/v0.5.0/packages/wraith/src/pool.ts#L23)

CSP alone does not establish a requirement for a separate application window. Serving the worker and Wasm assets and initializing WebGPU remain integration conditions. An application window and a `<webview>` are other placements. [inferred]

WebGPU on Intel through the engine's `ozone-drm` backend remains unestablished by the terminal studies. A working browser terminal under `ozone-wayland` does not establish that path. [#1882](https://github.com/cprussin/domicile/pull/1882) adds the Vulkan loader for by-name lookup. A subsequent [public follow-up](https://github.com/cprussin/domicile/issues/1881#issuecomment-6705917473) identifies another library-resolution route in Dawn and records an Intel adapter in an isolated headless control. It leaves DRM presentation unverified. See [GPU and render backends](../06-features/engine-mechanisms/gpu-and-render-backends.md). [docs]

## Lineage

- A 2026-05 kitty study separated keyboard encoding, graphics transmission, synchronized output, hyperlinks, command marks and remote control. These are independent terminal capabilities, distinct from the Wayland protocols that carry the final window. [docs]
- The 2026-05 renderer study contrasted incremental main-screen output with a renderer that owns the alternate screen. It recorded image registration versus placement, capability probing, local-file versus remote chunked transport, and explicit image cleanup as separate mechanisms. These were source readings, without a real-terminal exercise of those paths. [docs]
- The 2026-05 evaluation method separated capability in the terminal, a caller that actually emits it, and the observable result. Source inspection and reachable behavior were recorded separately. [docs]
- The 2026-10-26 study distinguished terminal keyboard and graphics compatibility from Kitty's own window, overlay and text-export APIs. [docs]
- The 2026-11-04 integration study cataloged native terminal window behavior and browser embedding limits; current status follows the pinned source links above. [docs]

## Sources

- S-3f524db52413 · 2026-11-09 · Compositor globals, seat resources and keyboard construction.

- S-425cf9befbb2 · 2026-11-09 · Pinned sway keyboard repeat defaults.

- S-ab93695e401f · 2026-07-18 · Terminal mouse-binding defaults.

- S-997e756f69fc · 2026-10-24 · Browser-terminal release architecture.

- S-5083a52eb56d · 2026-11-09 · Public Vulkan library-resolution follow-up, checked on this date.

- S-18c9c5046401 · 2026-10-24 · Browser-terminal module worker construction.

- S-23c521755f22 · 2026-11-09 · Native-window insertion and placement contract.

- S-0062bc389108 · 2026-11-08 · Software echo comparison, repeated local cohort.
- S-27501c9817a7 · 2026-07-18 · Kitty 0.48 rendering and benchmark contracts, pinned release documentation.
- S-42ecb235c7f2 · 2026-10-26 · Terminal protocol and clipboard boundary audit.
- S-623d8e5fefeb · 2026-11-07 · Initial terminal software echo study.
- S-63da10a11f44 · 2026-05-20 · Terminal renderer and graphics transport study.
- S-6ae15bd6541f · 2026-11-08 · Software echo comparison, original cohort.
- S-6ee2a1600602 · 2026-11-09 · Shell module security and packaging.
- S-882cc3aa0f38 · 2026-05-29 · Terminal protocol evaluation method.
- S-9b6eacc885db · 2026-07-18 · Kitty 0.48 Wayland registry binding, pinned release source.
- S-a886bb84a90d · 2026-11-09 · Desktop-entry launch filtering.
- S-c3ab182267b1 · 2026-11-04 · Native and browser terminal integration study.
- S-fd20eb1c6dd6 · 2026-11-09 · MIME application filtering.
