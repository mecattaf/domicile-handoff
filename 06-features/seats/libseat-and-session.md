---
id: seats/libseat-and-session
title: Device access and the tty session
kind: feature
family: seats
layers: [engine:input, engine:drm, session, external]
domicile_status: by-design-elsewhere
domicile_refs: [cprussin/domicile#1896, cprussin/domicile#1922]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, niri]
rows: []
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:seats, status:by-design-elsewhere, evidence:source, scroll-parity:baseline]
sources: [S-069533fa6b43, S-1951a846e891, S-1b21754fae3d, S-27f028d58ee2, S-2fcac3a67284, S-3335cd2786c6, S-3b0214711dda, S-41f16263b0fb, S-5ed837c45b96, S-633706e78879, S-6e3a39c8f860, S-866b9f3f829d, S-9298e590438f, S-9ba51a0237c8, S-b056064c5b44, S-bafc4a630b63, S-f152f60cb06a]
---
# Device access and the tty session

Device access is a session service concern. Domicile obtains display and input descriptors from logind; that choice is separate from how many logical Wayland seats a compositor advertises. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/TTY-SESSION.md#L7) [docs]

## Behavior

- One logind control connection authorizes both DRM cards and input devices. Releasing control returns all devices. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/TTY-SESSION.md#L9) [docs]
- A console switch follows logind activation and pause/resume signals. The engine does not replace logind VT handling or fall back to opening unbrokered input nodes. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/TTY-SESSION.md#L56) [docs]
- Revoked input descriptors are removed and reopened through the input factory. Native keyboard, touchpad and mouse descriptors then share libinput processing. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/TTY-SESSION.md#L88) [docs]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Named-seat device attachment commands | Logical grouping is separate from choosing a device-access broker | [pinned input manual](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-input.5.scd) |
| scroll | Retains the sway session and logical-seat machinery | `seat attach` assigns input after device access is obtained | [pinned manual](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll-input.5.scd) |
| libseat 0.9.1 documentation | Common device API over seatd, (e)logind and embedded seatd | Compiled backends are selected automatically or through `LIBSEAT_BACKEND` | [Library contract](https://github.com/kennylevinsen/seatd/blob/0.9.1/README.md#L17) [docs] |

## What the notes found

- The earlier session study identifies three independent responsibilities: brokered device access, event acquisition and Wayland seat delivery. S-9298e590438f · 2026-06-06 · Linux session and input acquisition study [docs]
- libseat can abstract logind and seatd device access. Using that abstraction does not itself create another pointer, keyboard focus or selection. [docs]
- 2026-11-05: Domicile takes DRM cards and input devices from logind through Chromium's D-Bus bus and documents no libseat; libseat would replace the engine's logind session, input and VT-switch classes, while sway pairs libseat with separate seat attach logic (sway) [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/A-DESKTOP-ON-A-TTY.md#L136> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-input.5.scd>
- 2026-11-04: As a direct session, the engine takes displays and input through logind with an Ozone DRM backend, with no outer compositor underneath; nested operation presents the desktop as one host window [docs] <https://github.com/cprussin/domicile/blob/08f0687dafa1018a9dcd7f227f43cc6dc557ffec/docs/architecture/A-DESKTOP-ON-A-TTY.md>
- 2026-06-29: The June session helper audit separates bus thread D-Bus work from origin thread callbacks. The draft leaves fresh resume descriptor handling unresolved; discarding that descriptor is not a validated session rule (Chromium historical studies) [docs]
- 2026-06-29: The uncompiled June seat review retains a stale device acquisition patch, missing installer wiring and unresolved resume-descriptor semantics; its shared-session design does not qualify VT recovery or DRM ownership (CUBS@2026-06-29) [docs]
- 2026-10-06: The early session study distinguishes active terminal-session identity from a lingering user manager’s environment and describes compositor-mediated terminal switching. Its inspection did not launch the compositor or exercise recovery (otto@v1.6.0) [docs]
- 2026-11-07: As of 2026-11-07 the engine obtains display and input devices from logind rather than libseat or seatd, so a sway-style libseat multi-seat setup would change how the engine owns display and input [docs]

## Where it touches Domicile

- `engine:drm` uses logind session control and device grants; logind owns DRM master. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/TTY-SESSION.md#L25) [docs]
- `engine:input` uses only authorized descriptors. The documented design explicitly uses Chromium D-Bus and no libseat. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/A-DESKTOP-ON-A-TTY.md#L128) [docs]
- Status at the pin: by-design-elsewhere for the libseat/seatd route. logind supplies this role in the pinned implementation. Hardware input after a full console round trip remains unverified in the upstream status table. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/A-DESKTOP-ON-A-TTY.md#L33) [docs]

- Device opens are serialized by device number. A canceled reply is cleaned up before a newer claim begins. Counted release echoes are separated from physical unplug, and stale resume descriptors do not replace a newer converter. [source] [Device claims](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/ozone/platform/drm/domicile/drm_input_opens.cc#L26) · [Release and resume](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/ozone/platform/drm/domicile/drm_input_devices.cc#L84)

## Lineage

- 2026-06-06: the CADE session study traces the Ozone device-broker seam separately from Exo seat delivery. S-9298e590438f · 2026-06-06 · Linux session and input acquisition study [docs]
- 2026-11-09: the pinned tty contract assigns card/input access and VT changes to logind. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/TTY-SESSION.md#L7) [docs]

## Sources

- S-633706e78879 · 2026-11-09 · Sway seat input configuration contract.

- S-9ba51a0237c8 · 2026-11-09 · libseat release 0.9.1 backend-selection contract.

- S-6e3a39c8f860 · 2026-11-09 · Logind release and resume device lifetime.

- S-5ed837c45b96 · 2026-11-09 · Serialized logind input claims.

- S-1951a846e891 · 2026-11-09 · Direct-session architecture and native device ownership.
- S-9298e590438f · 2026-06-06 · Linux session and input acquisition study.
- S-f152f60cb06a · 2026-11-09 · Logind card, input and console lifetime contract.
