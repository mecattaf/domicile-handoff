---
id: browser-windows/chrome-dialogs-in-shell
title: Browser prompts, pickers and error pages in the shell
kind: feature
family: browser-windows
layers: [engine:browser, sdk, manganese]
domicile_status: landed-with-gap
domicile_refs: [cprussin/domicile#1893, cprussin/domicile#1895, cprussin/domicile#1899, cprussin/domicile#1900, cprussin/domicile#1901, cprussin/domicile#1903, docs/architecture/ENGINE-BROWSER-BEHAVIOR.md, docs/KNOWN-GAPS.md]
checked_at: "2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc"
answered_upstream: false
scroll_parity: n/a
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:browser-windows, status:landed-with-gap, bucket:domicile, evidence:source]
sources: [S-14b41dc319ce, S-17b9826676ac, S-2d41418b00d5, S-2e2f8b81a108, S-4f0b6cbe710c, S-5ac26707cdda, S-695f43c6be84, S-7b2e0f983503, S-8100b35ed50a, S-866b9f3f829d, S-98a3ab4519cb, S-bcc52862f645, S-bf0ee3e14cf8, S-fa3bee869e4f, S-fcddb6547d9d]
donors: [views]
eras: [views-shell]
rows: [AL-views-inventory-167, AL-views-inventory-169]
depends_on: []
reported_on_domicile: []
---
# Browser prompts, pickers and error pages in the shell

A browser guest reports requests to its owning shell element. The shell supplies the interaction while Chromium retains browser decisions and request lifetime. [Browser interaction boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-BROWSER-BEHAVIOR.md#L99). [docs]

## Behavior

- JavaScript dialogs are serialized; navigation and close withdraw the pending dialog.
- Before-unload requests can delay a shell-initiated close.
- Device controllers retain device discovery and grant the selected device.
- Page pickers, tooltips, HTTP sign-in and load failures reach the shell.
- The [native Chrome modal adapter](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/chrome/browser/domicile/domicile_chrome_dialogs.cc#L23) enables refusal of native modal widgets. [source] Unsupported floating browser sheets remain explicitly disabled. [Dialog coverage](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-BROWSER-BEHAVIOR.md#L99). [docs]

## How donors do it

| Donor @ pin | Mechanism | Notable boundary | Link |
|---|---|---|---|
| Chromium @ 150 | Linux portal file selector | Linux host route differs from the ChromeOS Files application | [Portal selector](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ui/shell_dialogs/select_file_dialog_linux_portal.cc) |
| Domicile @ checked pin | Guest events and browser override hooks | A guest has no independent Chrome frame for an anchored native bubble | [Guest interaction hooks](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-BROWSER-BEHAVIOR.md#L56) |

## What the notes found

- The October census does not identify a portable native Ash file selector; it distinguishes Linux portal integration from the ChromeOS Files application. S-17b9826676ac. [docs]
- 2026-11-08: Manganese BrowserWindow wires permission, device, JavaScript-dialog and file-picker handlers; a bare webview embedding does not, and unhandled requests are dismissed. Chrome-native dialogs outside the handled set are refused, logged and canceled [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/BrowserWindow.tsx> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-BROWSER-BEHAVIOR.md>
- 2026-11-07: Main had merged PR 1903 (Chrome dialogs refused) and PRs 1893, 1895 and 1898 to 1901 (prompts and pages drawn in the shell). [docs] <https://github.com/cprussin/domicile/pull/1903> <https://github.com/cprussin/domicile/pull/1893>
- 2026-11-07: As of 2026-11-07 PR #1903 moves every Chrome-drawn prompt to the shell; payment sheets, sign-in sheets and picture-in-picture are switched off, remaining requests are refused and logged, and a guard test checks it [docs] <https://github.com/cprussin/domicile/pull/1903>

## Where it touches Domicile

- `engine:browser`: overrides route requests to the guest element; navigation, cancellation and close remain tied to the request. [Browser requests](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-BROWSER-BEHAVIOR.md#L99). [docs]
- `sdk` and shell: shell event handlers render and answer the requests.
- Status at the pinned checkout: `landed-with-gap`; no picture-in-picture video, Payment Request sheet or FedCM account sheet. These are documented limits rather than donor omissions. [Known browser limits](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md#L44). [docs]

- The shell-routing changes are merged before the checked pin: [application links #1893](https://github.com/cprussin/domicile/pull/1893), [HTTP sign-in #1895](https://github.com/cprussin/domicile/pull/1895), [error pages #1899](https://github.com/cprussin/domicile/pull/1899), [tooltips and page-state prompts #1900](https://github.com/cprussin/domicile/pull/1900), [pickers #1901](https://github.com/cprussin/domicile/pull/1901), and [remaining dialog refusal #1903](https://github.com/cprussin/domicile/pull/1903). [docs]

- A before-unload dialog [requests focus for its browser window](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/BrowserWindow.tsx#L411). While a shell-owned request is pending, the [focus path targets its waiting element](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/BrowserWindow.tsx#L530) and reclaims it if the guest takes focus again. This establishes prompt routing and keyboard ownership; it does not qualify every browser dialog type. [source]

- The browser guest-dialog guard allows one active request, binds its answer to a request generation and ignores stale replies; cancellation consumes the pending callback with a negative answer. [source] [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/guest_dialogs.cc#L16).

- Blink dispatches a cancelable JavaScript-dialog event and applies its default when unclaimed: before-unload accepts leaving, while alert, confirm and prompt default to dismissal. Claimed events remain held until answered or withdrawn. [source] [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_web_view_element.cc#L1029), [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/domicile_dialog_event.cc#L72), [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/domicile_dialog_event.idl#L28).

- Blink rejects repeated answers after a dialog is answered or withdrawn with InvalidStateError; withdrawing a held event dismisses it and emits a separate notification. [source] [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/domicile_dialog_event.cc#L51), [Pinned implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_web_view_element.cc#L1053).

## Lineage

- The October census compared browser-managed requests with system-level shell UI.

## Sources

- S-17b9826676ac · 2026-10-01 · Chromium window-system and shell seam census
- S-fa3bee869e4f · 2026-11-09 · Pinned browser interaction contract
- S-8100b35ed50a · 2026-11-09 · Pinned browser limits
- S-14b41dc319ce · 2026-11-09 · Domicile browser-window prompt and keyboard-focus routing
- S-2d41418b00d5 · 2026-11-09 pin check · Chromium 150 Linux portal availability and host routing
- S-2e2f8b81a108 · 2026-11-09 · Domicile JavaScript-dialog generation, default response and withdrawal path
- S-bf0ee3e14cf8 · 2026-11-09 · Domicile JavaScript-dialog generation, default response and withdrawal path
- S-fcddb6547d9d · 2026-11-09 · Domicile JavaScript-dialog generation, default response and withdrawal path
- S-4f0b6cbe710c · 2026-11-09 · Domicile JavaScript-dialog generation, default response and withdrawal path
- S-7b2e0f983503 · 2026-11-09 · Domicile native Chrome modal-widget refusal adapter
- S-98a3ab4519cb · 2026-11-09 · Domicile native Chrome modal-widget refusal adapter
