---
id: sway-family/scroll-parity/idle-and-lock
title: Idle and lock
kind: family
family: session
layers: [compositor, protocol, manganese, session, sdk, engine:input, engine:drm, engine:viz]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
scroll_parity: baseline
donors: [sway, scroll]
evidence_mix: mostly-source
bucket: domicile
tags: [scroll-parity:baseline, donor:sway, donor:scroll, bucket:domicile, evidence:source]
sources: [S-124834340237, S-204a969601d0, S-23c521755f22, S-3d1446ae145e, S-3f17426ddbf7, S-4a2b900f6a8d, S-d5b15decb594]
---

# Idle and lock

11 source rows are indexed in the [parity table](rows/scroll-parity.tsv). Option atoms are classified separately from changed parent commands. Shared interfaces and accounting aliases do not establish runtime equivalence. Counts are derived from the row table. [docs]

- Relations: 11 same; 0 scroll-changed; 0 scroll-only; 0 sway-only.
- Domicile coverage: 0 landed; 4 partial; 3 absent; 4 unverified; 0 accounting aliases/exclusions.
- Mechanism layers: compositor 10; engine:drm 1; engine:input 2; engine:viz 1; manganese 8; protocol 5; sdk 2; session 9.
- Feature: `session/idle-and-lock`.

## Donor semantics

Per-window idle inhibition can follow focus, fullscreen, visibility or window lifetime. Locker presentation and idle actions are separate from compositor lock enforcement. Crash-safe lock state, rejecting a second lock and restoring focus after unlock are independent native contracts. [source] [Idle policies](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L584).

The row map leaves native enforcement unverified where only shell documentation is available. A lock command or a screen-on bar toggle is evidence for that interface, rather than proof of every lock lifecycle invariant. [source] [Manganese commands](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L63).

Current native inhibition has two sources: live Wayland surfaces in the displayed-window set, and separately stored portal/application holds. Their combined veto controls both blanking and automatic locking. The per-input-source idle selector in the donor remains a different configuration contract. Hotplug restates disabled connectors only while idle blanking is dark, independently of lock state. New shell pages receive current lock state, and successful unlock broadcasts it. The allowed keyboard-focus message keeps page and compositor focus in agreement; it does not establish restoration of an earlier saved focus chain or per-output lock-surface coverage. [source] [Inhibitor scope](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/idle.rs#L154) · [Combined veto](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/idle.rs#L366) · [Dark hotplug guard](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/blanking.rs#L174) · [State delivery](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/locking.rs#L95) · [Focus agreement](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/LOCK.md#L35).

The mounted lock UI follows the current display list, with a whole-page fallback before that list arrives. Entry focuses the passphrase field and traps Tab, while bar panels may retain focus; unlock clears entered text and refusal state. The local lock mirror initially subscribes from unlocked, while native input/action gates enforce locking independently. These UI/state mechanisms do not prove secure presentation order on a newly attached display or donor-style inactive-focus restoration. Donor hotplug attaches the lock scene before requesting modesetting; donor unlock restores inactive focus, rearranges layers and re-evaluates inhibition. [source] [Lock coverage](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-lock/src/Lock.tsx#L118) · [Lock coverage](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-lock/src/Lock.tsx#L140) · [Lock coverage](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-lock/src/Lock.tsx#L300) · [State mirror](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-lock/src/useLocked.ts#L40) · [State mirror](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-lock/src/useLocked.ts#L52) · [State mirror](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/lock.rs#L330) · [Entry and cleanup](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-lock/src/Lock.tsx#L93) · [Entry and cleanup](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-lock/src/Lock.tsx#L120) · [Donor hotplug](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/desktop/output.c#L596) · [Donor hotplug](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/desktop/output.c#L620) · [Donor hotplug](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/lock.c#L167) · [Donor unlock](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/lock.c#L215) · [Donor unlock](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lock.c#L215).

## At the pin

Not advertised at the pin: the pinned compositor registration advertises idle-inhibit and no idle-notify or session-lock global; individual request semantics are not inferred from another protocol. [source] [Current implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/main.rs#L793).

## Sources

- [sway at the donor pin](https://github.com/swaywm/sway/tree/1652c54b73f67df17b7b4ab0b0f7048204aa8104).
- [scroll at the donor pin](https://github.com/dawsers/scroll/tree/b6586d273fed7d187744b7eaec2c234386a029fa).
- S-204a969601d0 · 2026-11-06 · sway and scroll semantic inventory.
- S-d5b15decb594 · 2026-11-09 · Manganese command guide at the pin.
- S-23c521755f22 · 2026-11-09 · Manganese window behavior at the pin.
- S-124834340237 · 2026-11-09 · Domicile shell and typed-state reference at the pin.
- S-3d1446ae145e · 2026-11-05 · Scroll reference at the donor revision.
- S-3f17426ddbf7 · 2026-09-21 · sway reference at the donor revision.
- S-4a2b900f6a8d · 2026-11-05 · Scroll reference at the donor revision.
