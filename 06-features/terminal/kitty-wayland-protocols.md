---
id: terminal/kitty-wayland-protocols
title: Kitty as an ordinary Wayland client
kind: feature
family: terminal
layers: [engine:input, compositor, protocol, manganese, external]
domicile_status: unknown
domicile_refs: []
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [kitty, sway, scroll]
rows: [fractional-scale-v1, primary-selection-v1, wl_compositor, wl_seat, xdg-decoration-v1, xdg-shell]
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:terminal, layer:protocol, donor:kitty, evidence:source, scroll-parity:baseline]
sources: [S-11402dc437c0, S-27501c9817a7, S-3f524db52413, S-425cf9befbb2, S-42ecb235c7f2, S-446385a8309f, S-5098aab151f1, S-63da10a11f44, S-695f43c6be84, S-882cc3aa0f38, S-93bfe16ec4f6, S-9b6eacc885db, S-a685462e855c, S-a886bb84a90d, S-ab93695e401f, S-af786326abd1, S-c3ab182267b1, S-cc75fef59e1e, S-cd452d4f1b50, S-fbe8a9d8422d, S-fd20eb1c6dd6]
---
# Kitty as an ordinary Wayland client

Kitty owns its terminal state, renderer, selection and child process. Domicile manages the native toplevel, supplies input and presents its buffers. A new terminal toplevel follows the shell's window policy; a Kitty split remains inside the terminal. [docs]

## Behavior

- Required registry globals establish a native client window and input seat. Optional bindings cover fractional scaling, cursor shapes, shortcut inhibition and selections. Each global has its own availability and version handling. The [coverage map](../../03-wayland-protocols/coverage-map.md) records the pinned advertisement separately from Kitty's optional bindings. [docs]
- The terminal's content rectangle determines columns and rows. A change in decoration, gaps, scale or fullscreen allocation changes the available grid without changing Kitty's terminal protocol. [docs]
- Clipboard selection and PRIMARY are distinct. Selection copying, ordinary paste and middle-click paste depend on Kitty's own actions and the advertised selection protocols. See [clipboard and drag-and-drop](../clipboard-dnd/data-control-and-primary-selection.md). [docs]
- Shell launch commands and desktop entries have different parsing contracts. At the pin, `Terminal=true` entries are omitted from both the launcher inventory and MIME handler selection. [source] [Launcher](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-apps/src/desktop-entry.ts#L36), [MIME handlers](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/mime_apps.rs#L96).

## How donors do it

| Donor @ pin | Mechanism | Observable boundary | Link |
|---|---|---|---|
| Kitty @ 0.48.0 | Wayland registry bindings in its GLFW backend | Required and optional bindings have distinct availability and version checks | [Registry binding](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_init.c#L580) |
| Kitty @ 0.48.0 | Per-window screen and local child ownership | Terminal state and process state are retained by the native client | [Terminal window](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/kitty/window.py#L775) |
| Domicile @ current pin | Desktop-entry parsing | A terminal wrapper is not configured, so `Terminal=true` is skipped | [Pinned parser](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-apps/src/desktop-entry.ts#L36) |

## Terminal capabilities and the window transport

| Capability | Mechanism | Domicile boundary | Evidence |
|---|---|---|---|
| Kitty keyboard protocol | Application opts into terminal key encoding through CSI sequences | Distinct from wl_keyboard delivery to the native client | [source] [Pinned implementation](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/docs/keyboard-protocol.rst#L87) |
| Kitty graphics protocol | Application sends raster graphics to the terminal emulator | The compositor receives the final client window buffer | [source] [Pinned implementation](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/docs/graphics-protocol.rst#L4) |
| Kitty remote control | Opt-in DCS commands or an optional UNIX/TCP socket | The defaults disable remote control and configure no listening socket; this remains separate from the compositor's own input path | [source] [Command family](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/docs/misc-protocol.rst#L55) · [Control default](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/kitty/options/definition.py#L2262) · [Socket default](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/kitty/options/definition.py#L2293) |
| Terminal throughput benchmark | Default benchmark suppresses screen rendering | Parser throughput does not establish display latency | [source] [Pinned implementation](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/docs/performance.rst#L90) |
| Pointer gesture hold | Wayland hold begin cancels local momentum | Client behavior remains separate from compositor gesture transport | [source] [Pinned implementation](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_init.c#L482) |
| Keyboard repeat information | A positive Wayland repeat_info rate enables the client timer; only repeatable keys arm it | Tracked release and focus loss stop it; delivery checks focus and a nonzero rate | [source] [Repeat information](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_init.c#L462) · [Timer](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_init.c#L342) · [Focus loss](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_init.c#L398) · [Delivery gate](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_window.c#L1336) |
| First Wayland seat | Registry handler binds a seat only before one is stored | Multiple server seats do not prove multiple independent client participants | [source] [Pinned implementation](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_init.c#L604) |

The [versioned capability contracts](capability-contracts.tsv) retain detailed keyboard-mode, graphics acknowledgment, MIME-transfer and rendering-observer limits for Kitty 0.48.0.

## What the notes found

- The terminal capability inventory distinguishes keyboard encoding, graphics transmission, synchronized output, OSC hyperlinks, prompt marks and remote control. A terminal can implement some of these without implementing Kitty's text-export or overlay API. [docs]
- A transport that forwards ordinary terminal bytes and a renderer that reconstructs those bytes from a second terminal model have different fidelity boundaries. Attached forwarding and a fresh window's restored state are separate claims. [docs]
- Exact terminal row counts depend on font cell dimensions, padding and the content rectangle. Counts from one display configuration are documentary arithmetic, not a universal Domicile result. [docs]
- 2026-11-06: Closing a Kitty window on Domicile aborted while freeing the Wayland output list (a count underflow); a backported upstream Kitty fix removed the abort. [measured]
- 2026-11-09: L-30 records that launcher and MIME candidate lists omit Terminal=true desktop entries (Domicile@pin) [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-apps/src/desktop-entry.ts#L36> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/mime_apps.rs#L96>
- 2026-11-07: Kitty 0.48.0 source uses touchpad hold gestures to stop momentum scrolling and has no pinch binding, so restoring pinch transport in the compositor leaves Kitty's response to client policy (kitty@0.48.0) [docs]

## Where it touches Domicile

- **Protocol/compositor:** the pinned native-client surface is listed in the [protocol map](../../03-wayland-protocols/coverage-map.md). Advertisement, server version and individual request behavior remain separate. [docs]
- **Engine input:** the compositor advertises keyboard repeat rate and delay; the client timer applies them. The [terminal overture](../../00-overtures/terminal.md#what-changes-around-the-window) gives the pinned delay comparison. Repeat onset remains separate from initial keypress delivery. [source] [Pinned keyboard setup](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L844).
- **Manganese:** [window groups and title bars](../browser-windows/tab-groups-and-strips.md), [focus policy](../windows/focus-and-urgency.md), [floating rules](../windows/criteria-and-window-rules.md) and [fullscreen display choice](../outputs/fullscreen-display-choice.md) determine what surrounds the terminal. [docs]
- **Host/system applications:** terminal-marked desktop entries remain excluded as of the pin. Direct executable launch does not go through that terminal-wrapper path. [source] [Launcher exclusion](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-apps/src/desktop-entry.ts#L53).
- Status: protocol advertisement is source-backed in the map. Terminal-wrapper discovery is not addressed as of the pin. Full Kitty behavior is not established by the protocol list alone. [source] [MIME-handler exclusion](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/mime_apps.rs#L96).

The renderer’s document-to-native producer consumes keydown and drops browser repeat events before sending a pressed Key request. Page-taken presses are tracked for release suppression. [source] [Keydown producer](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1302) · [Repeat guard](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/modules/domicile/domicile_host.cc#L1323)

## Lineage

- The 2026-05 kitty study separated terminal wire capabilities, rendering ownership and source-level verification. [docs]
- A 2026-10-26 audit distinguished keyboard and graphics compatibility from a complete Kitty API surface. [docs]
- A 2026-11-04 study mapped the terminal to native window, input, selection and launcher boundaries. Current status uses the pinned sources linked above. [docs]

## Sources

- S-fbe8a9d8422d · 2026-11-09 · Document-to-native key production and repeat filtering.

- S-ab93695e401f · 2026-07-18 · Kitty remote-control and listening-socket defaults.

- S-93bfe16ec4f6 · 2026-07-18 · Kitty keyboard repeat focus and rate gate.

- S-3f524db52413 · 2026-11-09 · Compositor globals, seat resources and keyboard construction.

- S-a685462e855c · 2026-07-18 · Local PTY creation and process spawning.

- S-11402dc437c0 · 2026-07-18 · Terminal window screen and local child ownership.

- S-425cf9befbb2 · 2026-11-09 · Pinned sway keyboard repeat defaults.

- S-cc75fef59e1e · 2026-07-18 · Pinned terminal capability implementation.

- S-5098aab151f1 · 2026-07-18 · Pinned terminal capability implementation.

- S-cd452d4f1b50 · 2026-07-18 · Pinned terminal capability implementation.

- S-27501c9817a7 · 2026-07-18 · Kitty 0.48 rendering and benchmark contracts, pinned release documentation.
- S-42ecb235c7f2 · 2026-10-26 · Terminal protocol and clipboard boundary audit.
- S-63da10a11f44 · 2026-05-20 · Terminal renderer and graphics transport study.
- S-882cc3aa0f38 · 2026-05-29 · Terminal protocol evaluation method.
- S-9b6eacc885db · 2026-07-18 · Kitty 0.48 Wayland registry binding, pinned release source.
- S-a886bb84a90d · 2026-11-09 · Desktop-entry launch filtering.
- S-c3ab182267b1 · 2026-11-04 · Native and browser terminal integration study.
- S-fd20eb1c6dd6 · 2026-11-09 · MIME application filtering.
