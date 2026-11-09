---
id: session/portals-and-sandboxing
title: Portals and sandboxed clients
kind: feature
family: session
layers: [engine:browser, compositor, protocol, sdk, manganese, external]
domicile_status: landed
domicile_refs: [cprussin/domicile#1226]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, smithay]
rows: [security-context-v1]
evidence_mix: mostly-source
boundary_note: true
bucket: domicile
tags: [family:session, status:landed, evidence:source, scroll-parity:baseline]
sources: [S-220a8533ce51, S-225e47dd3e6a, S-28f5a342d5a7, S-2b83da449b94, S-33889728083c, S-43af2b3af183, S-4f4471cd2252, S-5716f5a671cf, S-5f315b39ac7c, S-71082eb3e001, S-76d1bafcdec5, S-826ba4b265eb, S-8a57c3b149f7, S-aad47d4e0b88, S-c2596d00b100, S-c9366e4f4442, S-dfd54405f57b, S-e7392f226f69, S-e9dee995522a]
---
# Portals and sandboxed clients

Portals connect application requests to desktop permission and selection flows. Wayland security-context metadata changes which privileged globals a sandboxed connection can see. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L478) [docs]

## Behavior

- ScreenCast, Screenshot, RemoteDesktop, Clipboard and InputCapture are distinct portal interfaces with different sessions and permissions. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L448) [docs]
- Sandboxed clients retain focused data-device copy/paste and portals. They do not see unrestricted data-control, direct image capture, screencopy or nested security-context creation. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L484) [docs]
- Context identity is supplied by the launcher that creates it. The upstream contract distinguishes that claim from process credentials when making a security decision. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L491) [docs]

- RemoteDesktop EIS touch input uses one contact to move the pointer and press the left button. Additional contacts are ignored until that contact ends. The advertised EIS touchscreen capability describes this emulation path; it is separate from hardware touch delivery through `wl_touch`. [source] [Touch translation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/translation.rs#L156) · [Headless translation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/translation.rs#L230) · [EIS device capabilities](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/devices.rs#L23)

- Revoking or removing an EIS client releases its recorded keys and buttons through the direct-request or headless-engine route. Direct-request cleanup also sends pointer leave. [source] [Cleanup dispatch](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis.rs#L90) · [Connection removal](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis.rs#L485) · [Direct cleanup](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/translation.rs#L185) · [Headless cleanup](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/translation.rs#L258)
- An EIS connection used by InputCapture receives input. After connection and binding handling, its dispatcher ignores incoming input requests rather than injecting them into the desktop. [source] [Receiver request gate](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis.rs#L505)

### InputCapture source boundaries

- InputCapture grants keyboard and pointer classes. It masks requested capabilities to those classes; the RemoteDesktop touch emulation above is a separate path. [source] [Capability mask](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/input_capture.rs#L83) · [Connection grant](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/input_capture.rs#L347)
- Its helper recognizes capture activation from forwarded motion carrying a placed-window identifier when the desktop point reaches a barrier. Barriers lie along outer display edges. Outdated zone serials are rejected, resetting zones clears barriers, and release ends emulation without a cursor-warp request. [source] [Activation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/capture.rs#L54) · [Barrier geometry](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/barriers.rs#L85) · [Zone serial](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/input_capture.rs#L136) · [Zone reset](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/input_capture.rs#L187) · [Release](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/capture.rs#L150)
- Receiver-frame timestamps sample the monotonic clock when sending the framed event. They describe compositor delivery, with hardware acquisition timing separate. [source] [Frame timestamp](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/capture.rs#L182)

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Security-context filtering hides privileged protocol globals | An unsandboxed connection remains distinct from a sandbox-labeled one | [pinned server](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/server.c#L143) |
| scroll | Retains sway privileged-global filtering | Virtual input and data access remain separate protocol grants | [pinned server](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/server.c#L144) |

## What the notes found

- RemoteDesktop portal support supplies a Linux portal input interface and EIS transport. It is not by itself an RDP or VNC server. [docs]
- 2026-11-09: Legacy absolute pointer motion and touch down/motion validate the device grant, then reject the nonexistent stream. TouchUp follows the granted emulation path; EIS separately advertises absolute-pointer and touch capabilities [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/remote_desktop.rs#L184> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/remote_desktop.rs#L281> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/remote_desktop.rs#L413> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/remote_desktop.rs#L528>
- 2026-11-09: EIS regions use display logical sizes and scales, shifted by the minimum desktop origin. Absolute points outside all displays are ignored; relative motion leaving all displays clamps to the prior display or, after removal, the first [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/desk.rs#L50> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/desk.rs#L66> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/desk.rs#L75> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/translation.rs#L302>
- 2026-11-09: InputCapture advertises keyboard and pointer capability bits. Session creation masks requests to that set, and its EIS connection grant contains those device classes; touchscreen emulation belongs to a separate RemoteDesktop path [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/input_capture.rs#L25> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/input_capture.rs#L83> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/input_capture.rs#L213> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/input_capture.rs#L347>
- 2026-11-09: InputCapture rejects barriers for an outdated zone-set serial. Its zone-reset path clears stored barriers; Release stops active device emulation without requesting a cursor warp [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/input_capture.rs#L136> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/input_capture.rs#L187> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/input_capture.rs#L307> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/capture.rs#L150>
- 2026-11-09: InputCapture activates from forwarded motion bearing a placed window ID when its desktop point reaches a barrier. Captured relative motion is the difference between successive reported points; barriers must lie along outer display edges [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/capture.rs#L54> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/barriers.rs#L85> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/barriers.rs#L100>
- 2026-11-09: RemoteDesktop Start returns device and clipboard grants, plus optional restore metadata, without stream nodes. ScreenCast SelectSources rejects a handle absent from its separate session map; this does not establish an empty streams array [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/remote_desktop.rs#L378> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/screencast.rs#L245>
- 2026-11-09: Direct EIS hit testing uses placed window bounds. Among overlapping candidates it prefers the keyboard-focused window, then smaller area; this geometry heuristic is separate from the shell’s stacking model [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/desk.rs#L109> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/translation.rs#L314>
- 2026-11-09: The active InputCapture diversion match consumes native pinch, swipe, hold and touch requests without producing an EIS payload. This is the scoped helper path, with keyboard/pointer grants and physical acquisition qualification separate [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/eis/capture.rs#L136> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/portals/input_capture.rs#L25>
- 2026-11-04: At a 2026-11-04 snapshot the RemoteDesktop portal is the Linux portal API (input devices, an EIS/libei socket, clipboard); no RDP or VNC server, dependency or planned integration appeared in the inspected tree [source] <https://github.com/cprussin/domicile/blob/61b11927de3b369440ef147fe24ae5b5a7113730/packages/domicile-compositor/src/portals/remote_desktop.rs#L1-L22>
- 2026-11-04: At a 2026-11-04 snapshot legacy RemoteDesktop absolute-pointer and touch methods always reject stream coordinates because a session has no associated screen stream; EIS absolute input is the supported route [source] <https://github.com/cprussin/domicile/blob/61b11927de3b369440ef147fe24ae5b5a7113730/packages/domicile-compositor/src/portals/remote_desktop.rs#L413-L423>
- 2026-11-04: At a 2026-11-04 snapshot engine-injected input is enabled for headless and DRM platforms alike (platform not nested), although some portal comments describe it as headless-only [source] <https://github.com/cprussin/domicile/blob/61b11927de3b369440ef147fe24ae5b5a7113730/packages/domicile-launch/src/spawn.rs#L181-L193>
- 2026-11-05: The distro portal configuration routes default to domicile and Secret to gnome-keyring [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/distro/share/domicile-portals.conf>
- 2026-11-05: Domicile settings docs state folder picking goes through the FileChooser portal OpenFile with directory, drawn by the shell; a page cannot learn a folder path from an input of type file [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SETTINGS.md#L82>
- 2026-11-09: The portal docs show one dialog at a time, oldest first, over the app that asked; captures stack over other dialogs [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L32>

## Where it touches Domicile

- `compositor` serves security context and filters globals through its client classification. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L478) [docs]
- `engine:browser` sends browser-window capture through ScreenCast and uses the selected browser source as a tab capture when appropriate. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L321) [docs]
- `manganese` supplies the request dialogs and active-sharing state described by the portal contract. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/PORTALS.md#L346) [docs]
- Status at the pin: landed. The distro routes the portal backend to Domicile and delegates Secret to the keyring provider. [portal routing configuration](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/distro/share/domicile-portals.conf#L1) [source]

## Lineage

- 2026-10-13: [Sandboxed Wayland global filtering](https://github.com/cprussin/domicile/pull/1226) merges upstream. [docs]

The protocol history is indexed by collection.

Typical clients: screenshot and recording tools, remote desktop, clipboard managers, accessibility and test automation.

## Sources

- S-28f5a342d5a7 · 2026-11-09 · InputCapture outer-edge barrier validation.

- S-8a57c3b149f7 · 2026-11-09 · InputCapture activation, event framing and release.

- S-e9dee995522a · 2026-11-09 · InputCapture grants, zone serials and release contract.

- S-76d1bafcdec5 · 2026-11-09 · EIS session and translation dispatch.

- S-225e47dd3e6a · 2026-11-09 · EIS device capability declarations.

- S-220a8533ce51 · 2026-11-09 · EIS pointer and touch translation.

- S-2b83da449b94 · 2026-10-13 · Sandboxed Wayland global filtering merge record.

- S-43af2b3af183 · 2026-11-09 · Pinned donor implementation locator.

- S-e7392f226f69 · 2026-11-09 · Pinned donor implementation locator.

- S-33889728083c · 2026-11-05 · Portal backend routing and clipboard source study.
- S-c2596d00b100 · 2026-11-09 · Portal interfaces, direct capture and sandbox boundary.
