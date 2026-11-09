---
id: session/idle-and-lock
title: Idle blanking and the screen lock
kind: feature
family: session
layers: [engine:input, engine:drm, compositor, protocol, manganese, session]
domicile_status: landed
domicile_refs: [cprussin/domicile#1548, cprussin/domicile#1609, cprussin/domicile#1829]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, smithay]
rows: [ext-idle-notify-v1, ext-session-lock-v1, idle-inhibit-v1]
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:session, status:landed, evidence:source, scroll-parity:baseline]
sources: [S-0f33ebc98d72, S-11cdc5265665, S-124834340237, S-179ac96a4030, S-1951a846e891, S-1b032775d560, S-1f72359e341b, S-25668d056dae, S-32784ffd9a8d, S-3606bb231e4a, S-3d1446ae145e, S-3d35b7bab54c, S-3f17426ddbf7, S-43af2b3af183, S-44568bb9f588, S-6893829b6195, S-71889715ec58, S-71c6b5b6fa74, S-84bf0cd42927, S-9a57cc2646c0, S-9bae645999b5, S-a103e36fe4b1, S-a70560fdc087, S-b0a6749c4fd0, S-d08c6b7e0439, S-e7392f226f69, S-f9374ced90d5]
---
# Idle blanking and the screen lock

The compositor counts real input, enforces the desktop lock and turns connectors off for idle blanking. The shell draws the lock screen while the compositor remains the security boundary. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/IDLE.md#L3) [docs]

## Behavior

- Blanking and locking count from the same activity timestamp and share inhibitors. Releasing an expired inhibitor acts immediately. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/IDLE.md#L8) [docs]
- Keyboard input, buttons, scrolls and actual pointer motion count as activity. Synthetic movement after layout changes does not keep the desktop awake. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/IDLE.md#L23) [docs]
- The lock refuses native-client input and protected actions across shell reloads or engine restarts. A successful password verification unlocks it; a logind unlock signal does not. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/LOCK.md#L11) [docs]
- A missing configured PAM service prevents entry into an unrecoverable lock and produces an upstream warning. Once locked, verifier errors keep it locked. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/LOCK.md#L111) [docs]

- The PAM verifier performs authentication only. It reuses the submitted passphrase for every prompt, so distinct password and one-time-code answers are outside this interface. [source] [PAM conversation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/pam.rs#L16) · [Prompt response](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/pam.rs#L335)
- Authentication runs off the compositor loop with one outstanding check. Changing the verifier invalidates an earlier result while retaining the refusal delay. Entering the lock releases held keys and cancels active pinch, swipe and touch; repeated lock requests do not repeat that transition. [source] [Verifier lifetime](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/lock.rs#L145) · [Lock transition](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/locking.rs#L31)

- The shell keeps the input needed to operate its lock screen. Readouts and the keymap, tint and arrangement calls explicitly allowed by the lock gate remain available. [source] [Lock request boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/lock.rs#L7) · [Allowed calls](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/lock.rs#L338)

- A blanked desktop restates disabled connectors after hotplug. That guard reads blanking state, which remains separate from the compositor lock state. [source] [Hotplug blanking guard](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/blanking.rs#L174) · [Connector restatement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/blanking.rs#L195)

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Unlock refocuses each seat through its inactive focus node | It rearranges output layers, rechecks idle inhibition and commits a transaction | [Unlock path](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/lock.c#L215) |
| scroll | Retains the per-seat unlock focus path | Layer arrangement and idle inhibition are recomputed at unlock | [Unlock path](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lock.c#L215) |

Sway and Scroll retain a replaceable lock state after losing the lock client and recolor the output cover rectangles. They refuse a second active locker. These are source lifecycle paths, without a physical-display qualification here. [source] [Sway client loss](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/lock.c#L245) · [Sway replacement gate](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/lock.c#L261) · [Scroll client loss](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lock.c#L245) · [Scroll replacement gate](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lock.c#L261)

## What the notes found

- The pinned design distinguishes lock enforcement from lock-screen rendering. A page-only lock would disappear on reload. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/LOCK.md#L13) [docs]
- 2026-11-09: Sway and Scroll keep a lost lock client’s state as replaceable and recolor output cover rectangles. A new locker may replace that state; a second active locker is refused (sway/scroll pinned references) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/lock.c#L245> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/lock.c#L261> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lock.c#L245> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lock.c#L261>
- 2026-11-09: Manganese’s mounted lock sheet covers the page and derives per-screen veils from the current display list. Before display data arrives, it uses a whole-page veil [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-lock/src/Lock.tsx#L118> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-lock/src/Lock.tsx#L140> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-lock/src/Lock.tsx#L300>
- 2026-11-09: Manganese’s lock-state mirror initializes unlocked and subscribes to compositor state. The compositor independently enforces locked input and protected actions; this initialization alone establishes no presentation timing result [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-lock/src/useLocked.ts#L40> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-lock/src/useLocked.ts#L52> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/lock.rs#L330>
- 2026-11-09: Both donor new-output handlers attach an active session-lock scene before requesting modesetting. The lock scene has its own opaque per-output background; this is source behavior without a new hardware qualification (sway/scroll pinned references) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L596> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L620> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/lock.c#L167>
- 2026-11-09: Chord registration accepts a chord string without lock-eligibility metadata; the complete SDK binding dispatcher and Manganese binding hook have no locked-state input or selective lock-safe binding option (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L749> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/bind-keys.ts#L59> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/keyboard/useKeybindings.ts#L17>
- 2026-11-09: Wayland idle inhibitors hold only for live surfaces present in the displayed-window set. Portal/application holds are stored separately, and the same combined veto controls both blanking and automatic locking [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/idle.rs#L154> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/idle.rs#L366>
- 2026-11-09: The lock gate permits KeyboardFocus while suppressing client input. Its documented purpose is agreement between page focus and compositor focus after unlock [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/lock.rs#L333> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/LOCK.md#L35>
- 2026-11-09: A newly connected shell page receives the compositor’s current lock state. Unlock verification broadcasts the resulting state to every connected page [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/locking.rs#L95> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/locking.rs#L145>
- 2026-11-09: On the lock transition, Manganese focuses its passphrase field and traps Tab; bar panels can keep focus. Its unlock effect clears the entered passphrase and refusal marker [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-lock/src/Lock.tsx#L93> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-lock/src/Lock.tsx#L120>
- 2026-11-09: While locked, Manganese shows no notification toasts or drawer, and the compositor refuses notification dismissal and action requests (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/NOTIFICATIONS.md#L126>
- 2026-11-09: Manganese aggregates active keep-awake holds by application or website, naming each owner once; the shell's own Keep awake toggle is separately removable (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/shell-manganese/docs/TOP-BAR.md#L427>
- 2026-06-29: CUBS draft coverage records an incomplete idle and inhibit producer while deferring lock and authentication integration; those marks describe plans rather than runtime behavior (Chromium historical studies) [docs]
- 2026-06-29: The June uncompiled session draft separates enum and boolean visibility gates from lock authentication producers; its constant unlocked state and no-op actions do not establish session-lock support (Chromium historical studies) [docs]
- 2026-06-29: The June session bridge corrective addendum separates direct lock authentication from polkit authentication and changes the planned bus ownership; these are draft corrections, with implementation and secure lock enforcement still unverified (Chromium historical studies) [docs]

## Where it touches Domicile

- `engine:input` reports activity even when browser content or the shell consumes it. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/IDLE.md#L23) [docs]
- `compositor` enforces the lock, manages inhibitors and emits current state to a reloaded shell. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/LOCK.md#L21) [docs]
- `engine:drm` blanks connectors through the ordinary display-layout path; the retained desktop resumes on relight. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/IDLE.md#L33) [docs]
- Status at the pin: landed. Idle-inhibit is advertised. The complete registration search finds no ext idle-notify or ext session-lock global; their absence does not imply missing native idle or lock behavior. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/IDLE.md#L83) [docs]

## Lineage

- 2026-10-27: [Browser wake-lock inhibition](https://github.com/cprussin/domicile/pull/1609) merges upstream. [docs]

- 2026-10-25: [Idle display power](https://github.com/cprussin/domicile/pull/1548) merges upstream. [docs]

- 2026-11-02: [PAM-service check before locking](https://github.com/cprussin/domicile/pull/1829) merges upstream. [docs]

- 2026-11-09: pinned upstream docs define compositor-enforced locking, visibility-aware inhibition and logind sleep ordering. [upstream documentation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/LOCK.md#L97) [docs]

## Sources

- S-11cdc5265665 · 2026-11-09 · Scroll lock and unlock focus lifetime.

- S-3d35b7bab54c · 2026-11-09 · Sway lock and unlock focus lifetime.

- S-1f72359e341b · 2026-11-09 · Blanking state and output hotplug.

- S-71889715ec58 · 2026-10-27 · Browser wake-lock inhibition merge record.

- S-0f33ebc98d72 · 2026-10-25 · Idle connector-power merge record.

- S-f9374ced90d5 · 2026-11-02 · Configured PAM-service validation merge record.

- S-84bf0cd42927 · 2026-11-09 · Input cancellation on lock transition.

- S-71c6b5b6fa74 · 2026-11-09 · Lock verifier lifetime and backoff.

- S-25668d056dae · 2026-11-09 · PAM authentication conversation scope.

- S-43af2b3af183 · 2026-11-09 · Pinned donor implementation locator.

- S-e7392f226f69 · 2026-11-09 · Pinned donor implementation locator.

- S-124834340237 · 2026-11-09 · Screen-lock enforcement and authentication contract.
- S-1951a846e891 · 2026-11-09 · Direct-session architecture and native device ownership.
- S-32784ffd9a8d · 2026-11-09 · Idle activity, blanking and inhibitor contract.
