---
id: session/xdg-activation-and-focus-requests
title: Activation tokens and focus requests
kind: feature
family: session
layers: [engine:browser, compositor, protocol, sdk, manganese]
domicile_status: landed
domicile_refs: [cprussin/domicile#1040, cprussin/domicile#1778]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: true
scroll_parity: baseline
donors: [sway, scroll]
rows: [xdg-activation-v1]
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:session, status:landed, evidence:source, scroll-parity:baseline]
sources: [S-014c456bb822, S-069533fa6b43, S-1f5025ab76ad, S-31faae11b19c, S-324564fc75cd, S-446385a8309f, S-4d168063b887, S-543465ec2563, S-573bb6c061de, S-58480e08aa90, S-5e6d5b910bc8, S-7194d751e6c4, S-7c02419c39c3, S-866b9f3f829d, S-a89a4c46b8f9, S-be5932a4f7a2, S-c4641774aeb3, S-e77ffff40807, S-f5d75cc3208b]
---
# Activation tokens and focus requests

Activation tokens distinguish a timely user-driven focus handover from a background request. The compositor validates the request and the shell applies accepted focus policy. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/activation.rs#L69) [source]

## Behavior

- A minted token lives for ten seconds; a later mint replaces it. Notification activation can invite only its sender while the token remains valid. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/activation.rs#L35) [source]
- Input-backed tokens are compared with the keyboard focus entry serial. A client currently holding focus can hand it to one of its own windows. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/activation.rs#L69) [source]
- A rejected activation makes the target urgent; an accepted request becomes a shell focus request. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/xdg_shell.rs#L349) [source]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Validates xdg activation and associates launches with a seat | Token ownership and eventual window focus are separate | [pinned activation handler](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/xdg_activation_v1.c#L40) |
| scroll | Retains sway activation handling | Shared workspace policy still determines visible placement | [pinned handler](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/xdg_activation_v1.c#L40) |

## What the notes found

- An app opening a new window, an activation token and a shell focus request are distinct events. A launch without a token does not demonstrate a token-validation result. [inferred]
- 2026-11-09: Exo stores activation capability on the window and exposes separate timed grants, indefinite grants, permission checks and explicit revocation (exo@domicile-engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_util.cc#L300>
- 2026-11-09: For ready Exo surfaces, activation requests require the security delegate’s approval; requests before readiness only record an initial-activation flag (exo@domicile-engine-pin) [source] <https://github.com/chromium/chromium/blob/2493812cc0621330895498ea437bde415944c203/components/exo/shell_surface_base.cc#L1331>
- 2026-11-06: Mutter rejects activation timestamps older than the latest user input and marks the window as demanding attention; untimed requests from ordinary windows on another workspace also take the attention path (mutter@b766717ac9ad04d44211557ec79a545dc8c44ee7) [source] <https://github.com/GNOME/mutter/blob/b766717ac9ad04d44211557ec79a545dc8c44ee7/src/core/window.c#L3974>
- 2026-11-06: A window started through the compositor command dispatcher landed on the current workspace and took keyboard focus. [measured]
- 2026-11-01: A 2026-11-01 merged change (#1778) adjusted xdg_activation serial handling so an app holding keyboard focus can pass it to another of its own windows; #1040 moved a shell urgent workaround into a compositor decision on token serials [docs] <https://github.com/cprussin/domicile/pull/1778> <https://github.com/cprussin/domicile/pull/1040>
- 2026-06-29: The June activation design distinguishes protocol XML and code generation from a server global. Its token store remained uncompiled, token lifetime and serial policy stayed open, and activation permission involved more than a direct focus call (Chromium historical studies) [docs]
- 2026-06-29: A June launcher correction treated activation-token generation and process launching as a separate surface/compositor contract. Its review left end-to-end launch dependent on an unlanded token seam and runner implementation (Historical Chromium launcher design) [docs]
- 2026-06-29: A June activation design reports a corrective code draft, while retaining checks for live input serials, requester destruction and launch-environment handoff. Protocol schema and code-generation target presence remain distinct from compiled or runtime support (Historical Exo activation design) [docs]
- 2026-11-06: On 2026-11-06 a terminal opened by a background process took keyboard focus on the current workspace; no activation-token path was involved [docs]
- 2026-11-07: Chrome's reuse of an existing process hides which launch produced which window; activation tokens were not observed in its Wayland activation requests [docs]

## Where it touches Domicile

- `compositor` consumes and validates activation state. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/xdg_shell.rs#L336) [source]
- `sdk` and `manganese` receive an accepted focus request instead of bypassing shell policy. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/xdg_shell.rs#L349) [source]
- Status at the pin: landed. The public history records [#1778](https://github.com/cprussin/domicile/pull/1778) refining same-client focus handover after [#1040](https://github.com/cprussin/domicile/pull/1040) moves serial policy into the compositor. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/activation.rs#L69) [source]

- The shell contract describes recent-input native focus requests and urgency for rejected requests. Compositor source above supplies the token and serial checks. [docs] [Native focus-request contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L616)

## Lineage

- 2026-11-01: the public history records same-client focus handover and compositor serial validation. S-a89a4c46b8f9 · 2026-11-05 · Activation token source and public history study [docs]
- 2026-11-09: pinned source enforces token lifetime and emits accepted focus requests or urgency. [pinned source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/activation.rs#L35) [source]

## Sources

- S-e77ffff40807 · 2026-11-09 · Native focus requests and urgency contract.

- S-1f5025ab76ad · 2026-11-01 · Focused-client handover exception merge record.

- S-014c456bb822 · 2026-10-09 · Compositor activation-serial validation merge record.

- S-573bb6c061de · 2026-11-09 · Pinned donor implementation locator.

- S-31faae11b19c · 2026-11-09 · Pinned donor implementation locator.

- S-069533fa6b43 · 2026-11-05 · Session device access and activation source study.
- S-a89a4c46b8f9 · 2026-11-05 · Activation token source and public history study.
- S-c4641774aeb3 · 2026-11-09 · Activation-token lifetime and serial validation.
- S-f5d75cc3208b · 2026-11-09 · Xdg fullscreen output and activation request routing.
