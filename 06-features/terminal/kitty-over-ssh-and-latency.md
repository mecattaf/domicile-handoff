---
id: terminal/kitty-over-ssh-and-latency
title: Local terminal rendering and remote SSH transport
kind: feature
family: terminal
layers: [engine:input, compositor, protocol, session, external]
domicile_status: unknown
domicile_refs: []
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [kitty]
rows: []
evidence_mix: mixed
boundary_note: false
bucket: domicile
tags: [family:terminal, donor:kitty, layer:external, evidence:docs, scroll-parity:baseline]
sources: [S-0062bc389108, S-083adea7e7ad, S-11402dc437c0, S-27501c9817a7, S-3476d0b80faf, S-3c67451059e1, S-4c9bb6a586e9, S-4ed3510bbb79, S-4ff714d542b2, S-623d8e5fefeb, S-6ae15bd6541f, S-866b9f3f829d, S-872f157b2013, S-8da3eb7bda3b, S-93bfe16ec4f6, S-9b6eacc885db, S-a51398af2248, S-a685462e855c, S-b9df7238ba4e, S-bb76c1075d0e, S-c176d5dc707b, S-c3ab182267b1, S-c40e302fa0da, S-db1e64398669, S-ec3cb0925ecc, S-f8d35c67eb87]
---
# Local terminal rendering and remote SSH transport

A local Kitty window renders the terminal while SSH carries bytes to and from a remote process. Local presentation, network delivery, remote PTY lifetime and application state remain separate contracts. The [terminal overture](../../00-overtures/terminal.md) connects them to the desktop. [docs]

## Behavior

- Ordinary SSH transports an interactive byte stream. It does not by itself preserve the remote PTY after the connection ends. A retained remote process and the state required to reconstruct its terminal screen are separate. [docs]
- A connected terminal retains its own modes, graphics placements and scrollback. A fresh terminal window starts with different local state, even when a remote process survived. [docs]
- Software echo observes when output bytes arrive. A physical input-to-pixel measurement includes additional keyboard, compositor and display stages. [docs]
- A quiet first key and a continuing burst can meet different SSH interactive timing policies. [docs]

- In Wraithterm 0.5.0’s command-line backend, each WebSocket connection owns a PTY. Closing that connection terminates its PTY; stopping the server terminates those still running. Other embeddings can choose a different lifetime. [source] [Versioned PTY backend](https://github.com/betterclever/wraithterm/blob/v0.5.0/packages/wraith/bin/wraithterm.mjs#L313)
- OpenSSH’s timing protection can schedule interactive keystrokes and send padding after typing stops. This explains why differently configured software paths stay separate; it does not identify the binary version or effective defaults used in the historical measurements. [source] [Versioned SSH timing contract](https://github.com/openssh/openssh-portable/blob/V_10_2_P1/ssh_config.5#L1495)

### Callback and presentation boundaries

| Source boundary | Timestamp and action | Evidence |
|---|---|---|
| Domicile frame callback | Samples the compositor clock at send time; timer fallback can answer without an engine presentation report | [source] [Callback send](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/frame_pacing.rs#L172) · [Timer fallback](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/frame_pacing.rs#L85) |
| Domicile presentation feedback | Uses the engine-supplied shown time for reported frames; timer fallback discards feedback for an unreported frame | [source] [Engine feedback](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/frame_pacing.rs#L155) · [Fallback](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/frame_pacing.rs#L85) |
| Kitty render readiness | Samples local monotonic time, marks rendering ready and schedules a tick; the helper can invoke readiness immediately while a swap awaits commit. The wire callback time argument is ignored | [source] [Dispatch](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_window.c#L3012) · [Immediate helper](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/glfw/wl_window.c#L3089) · [Readiness time](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/kitty/glfw.c#L2782) · [Render scheduling](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/kitty/child-monitor.c#L957) |

A frame callback by itself does not establish visible-pixel time. [inferred]

## How donors do it

| Donor @ pin | Mechanism | Observable boundary | Link |
|---|---|---|---|
| Kitty @ 0.48.0 | Terminal parser, retained screen and client renderer | Terminal state stays in the local client | [Retained terminal text](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/kitty/window.py#L775) |
| Kitty @ 0.48.0 | Per-window local command and PTY | Local process state is separate from the window and retained screen | [Child process](https://github.com/kovidgoyal/kitty/blob/5b4ea1bbba26a9a4ba74dc9a5bb587c9f983fec5/kitty/child.py#L337) |

## What the notes found

The recorded cohorts measure software byte arrival. They exclude physical input, Kitty GUI rendering, the compositor and the display. Local and network observers have different boundaries. No matched physical Sway-to-Domicile comparison is present. [docs]

The [terminal overture](../../00-overtures/terminal.md#ssh-carries-bytes-to-a-remote-host) contains the recorded local and network medians. The early raw observer, original local comparison and repeated local comparison remain separate cohorts. [docs]


Quiet first keys and continuing bursts remain separate cohorts. They do not establish a constant per-key policy penalty, and subjective responsiveness remains distinct from these software observers. [docs]
- 2026-11-06: Kitty 0.48.0 aborted on window close under Domicile; an upstream Kitty fix removed the abort, placing the cause in Kitty rather than Domicile (kitty@0.48.0) [measured]
- 2026-11-07: On an Intel laptop, kitty and a WebGPU-rendered terminal each sustained a paced 6,000-line output workload in about five seconds; kitty used roughly 11% of one core, the WebGPU terminal in the official engine about 24% and 523 MiB. The two acknowledgment paths differ [measured]
- 2026-10-23: Local GPUI terminal frames use shared memory; the remote path compresses complete RGBA frames into Kitty graphics escapes. Two outstanding frames bound queue depth, but synchronous writes can block rendering. End-to-end latency remains unmeasured (GPUIX@a53b294401280014e94eb9bd98a7c7ed1929f92d) [docs] <https://github.com/remorses/gpuix/blob/a53b294401280014e94eb9bd98a7c7ed1929f92d/packages/gpui-terminal/src/kitty.rs#L247> <https://github.com/remorses/gpuix/blob/a53b294401280014e94eb9bd98a7c7ed1929f92d/packages/gpui-terminal/src/window.rs#L679>
- 2026-11-04: Keystrokes to a Kitty window pass through the shell page main thread, so a heavy React render could add typing latency; the notes state it was not measured [inferred]
- 2026-11-08: A 2026-11 comparison of software echo over a remote terminal found the largest typing-latency effect came from SSH keystroke-timing obfuscation during sustained output; quiet first-key latency showed no consistent penalty. Physical key-to-screen latency was not measured [docs]
- 2026-11-08: Eternal Terminal keeps packet counters on surviving peers and replays missing packets after reconnect; its interactive path drops queued output on interrupt, so it is not an unchanged lossless carrier for terminal graphics bytes [docs]
- 2026-11-08: On a laptop running Domicile, software echo through kitty over default OpenSSH to a remote shell measured about 10 to 13 ms median during ongoing typing and 2 to 3.5 ms after a quiet interval; with interactive timing obfuscation off, about 2 to 3 ms. Software byte arrival, not display latency [docs]
- 2026-11-08: In tests the kitty ssh kitten bootstrap consumed a complete command typed before the handshake, while ordinary OpenSSH preserved the queued input [docs]
- 2026-11-08: Kitty keeps its own VT engine and renderer, so a remote shadow engine adds state for reattachment rather than replacing it; kitty 0.50 logout restore saves launch instructions and main-screen history, not the remote process, and keeps 100,000 local scrollback lines against 10,000 restored [docs]
- 2026-11-08: Fresh-window restoration after a reconnect can lose image placements, palette overrides, cursor shape and OSC 66 text sizing; saved keyboard stacks, images and scrollback are separate fidelity cases [docs]

## Where it touches Domicile

- **Protocol/compositor:** the local terminal remains an ordinary native client. Its window contract is recorded in [Kitty's Wayland integration](kitty-wayland-protocols.md). [docs]
- **Engine input:** native-client input delivery passes through the desktop's engine and compositor boundary. Repeat onset and initial keypress latency are distinct. See [keybindings](../keyboard-and-text/keybinding-model.md). [docs]
- **Session/external:** connection loss, remote process retention and fresh-screen reconstruction are outside the native-window protocol. [docs]
- Status: the sources establish documentary software-path measurements and a source map. They do not establish pinned Domicile input-to-pixel latency or complete reconnection fidelity. [docs]

## Lineage

- The 2026-11-07 initial study separated software PTY echo, network transport and subjective desktop responsiveness. [docs]
- The 2026-11-07–08 consolidation retained an original and repeated local cohort, separated ongoing from quiet keys, and compared SSH timing policies without treating them as interchangeable architectures. [docs]

## Sources

- S-a51398af2248 · 2026-07-18 · Kitty frame readiness and render scheduling.

- S-93bfe16ec4f6 · 2026-07-18 · Kitty Wayland frame callback dispatch.

- S-3c67451059e1 · 2026-07-18 · Kitty frame-ready callback and local readiness timestamp.

- S-083adea7e7ad · 2026-11-09 · Presentation feedback and timer fallback.

- S-b9df7238ba4e · 2026-11-09 · Frame callback lifetime.

- S-a685462e855c · 2026-07-18 · Local PTY creation and process spawning.

- S-11402dc437c0 · 2026-07-18 · Terminal window screen and local child ownership.

- S-872f157b2013 · 2026-11-09 · Interactive SSH packet scheduling.

- S-f8d35c67eb87 · 2026-10-24 · Browser-terminal command-line PTY lifetime.

- S-0062bc389108 · 2026-11-08 · Software echo comparison, repeated local cohort.
- S-27501c9817a7 · 2026-07-18 · Kitty 0.48 rendering and benchmark contracts, pinned release documentation.
- S-623d8e5fefeb · 2026-11-07 · Initial terminal software echo study.
- S-6ae15bd6541f · 2026-11-08 · Software echo comparison, original cohort.
- S-9b6eacc885db · 2026-07-18 · Kitty 0.48 Wayland registry binding, pinned release source.
- S-c3ab182267b1 · 2026-11-04 · Native and browser terminal integration study.
- S-db1e64398669 · 2026-11-07–08 · Terminal byte-arrival methodology and limits.
