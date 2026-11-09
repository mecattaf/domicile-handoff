---
id: families/session
title: session features
kind: family
family: session
layers: []
domicile_status: n/a
checked_at: "2026-11-09"
scroll_parity: n/a
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [bucket:domicile]
sources: [S-612a99e2b687, S-660caed2860b, S-873afc9d9e8b, S-a6155a1467c4, S-abc5cbccba04, S-c1ba86476ed8, S-cc4c17362ba5, S-e3b674100515]
---

# session features

Idle, lock, portal permissions, activation and component recovery.

Statuses at the pin: 5 landed.

- [Idle blanking and the screen lock](idle-and-lock.md).
- [Portals and sandboxed clients](portals-and-sandboxing.md).
- [Shell reload and component recovery](shell-reload-and-recovery.md).
- [Startup commands and shell reload](startup-commands.md).
- [Activation tokens and focus requests](xdg-activation-and-focus-requests.md).
- [Other recorded contracts](facts.tsv): source rows without a separate feature page.

## Other recorded contracts

| Behavior | Contract at the pin | Evidence |
|---|---|---|
| Application process scopes | With client scoping enabled, application argv is wrapped in a systemd user scope under app.slice. Without a requested scope, the spawn helper keeps the original argv; the launcher passes scope-clients conditionally. | [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/spawning.rs#L98) · [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/spawn.rs#L250) · [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor/src/app_scope.rs#L19) [source] |
| Rendering-thread permissions | System limits and the RTKit service belong to system configuration; a standalone Home Manager configuration does not grant them. | [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/RUNNING-A-DESKTOP.md#L299) [docs] |
| Configuration evaluation errors | An initial module-evaluation error propagates before desktop setup. Live module reevaluation stages and renames a replacement only after successful evaluation; raw JSON configuration follows a separate path. Visible error presentation remains unqualified. | [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/bin/domicile.rs#L614) · [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/bin/domicile.rs#L1310) [source] |
| Signal stop and exit status | SIGINT and SIGTERM handlers set the launcher stop marker. The stopped outcome maps to a nonzero exit status at the desktop entry point; this states source control flow. | [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/supervise.rs#L310) · [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/bin/domicile.rs#L820) [source] |
| Application launch environment | The launcher prepares the compositor environment with BROWSER set to its browser helper and runtime shims prepended to inherited PATH. This states environment construction without establishing URL-handler choice or successful client launch. | [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/spawn.rs#L194) · [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/spawn.rs#L221) · [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/spawn.rs#L260) [source] |
| Recoverable SDK subscriptions | After compositor loss, recoverable SDK subscriptions register under a new call identifier. Streams carrying a missed-events marker report that intervening changes were not heard; this does not establish complete application-state restoration. | [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/system-calls.ts#L635) [source] |
| HTTP and HTTPS routing | The launcher routes HTTP and HTTPS addresses directly to a browser window before considering a MIME application. | [source](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/open.rs#L139) [source] |

## Sources

Each feature links its pinned implementation and dated investigation sources.

- S-612a99e2b687 · 2026-11-09 · Recoverable SDK subscriptions source record.

- S-660caed2860b · 2026-11-09 · Signal stop and exit status source record.

- S-873afc9d9e8b · 2026-11-09 · Signal stop and exit status source record.

- S-a6155a1467c4 · 2026-11-09 · Application launch environment source record.

- S-abc5cbccba04 · 2026-11-09 · Application process scopes source record.

- S-c1ba86476ed8 · 2026-11-09 · Rendering-thread permissions source record.

- S-cc4c17362ba5 · 2026-11-09 · HTTP and HTTPS routing source record.

- S-e3b674100515 · 2026-11-09 · Application process scopes source record.
