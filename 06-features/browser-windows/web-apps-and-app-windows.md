---
id: browser-windows/web-apps-and-app-windows
title: Web apps and engine-owned app windows
kind: feature
family: browser-windows
layers: [engine:browser, sdk, manganese, session]
domicile_status: landed
domicile_refs: [cprussin/domicile#1125, cprussin/domicile#1179, cprussin/domicile#1692, docs/architecture/WEB-APPS.md, docs/SHELL-BROWSER-WINDOWS.md]
checked_at: "2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc"
answered_upstream: false
scroll_parity: n/a
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:browser-windows, status:landed, bucket:domicile, evidence:source]
sources: [S-0aa579827d73, S-14b41dc319ce, S-179ac96a4030, S-1cd4484a3a81, S-25cef72c3619, S-2fcac3a67284, S-5ac26707cdda, S-5c4621c3f33a, S-695f43c6be84, S-866b9f3f829d, S-9852d15a44e1, S-9d1497c20da4, S-bcc52862f645, S-cc179bf996c8, S-e5e4d7fe8e6a, S-e5f53635c8c6, S-fcddb6547d9d, S-fd0c87388c02]
donors: []
eras: [COWL, CUBS]
rows: [AL-rt-tree-findings-034]
depends_on: []
reported_on_domicile: []
---
# Web apps and engine-owned app windows

A web app is a URL with a desktop entry. Its engine-owned browser window survives shell reloads and carries app identity independently of how a shell places it. [Web app contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WEB-APPS.md#L3). [docs]

## Behavior

- App windows carry `isApp` and an optional app id.
- Reuse can open a new window, focus the oldest matching app window, or navigate that window.
- The engine matches reuse requests, avoiding races between launches and a shell's window list.
- New windows opened by the app are ordinary browser windows.
- A detached shell does not own the guest page's lifetime. [Window ownership](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-BROWSER-WINDOWS.md#L15). [docs]

## How donors do it

| Donor @ pin | Mechanism | Boundary | Link |
|---|---|---|---|
| Domicile @ checked pin | Engine-owned BrowserWindows and `<webview window>` | Profile, page and app identity outlive the shell document | [Browser window contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-BROWSER-WINDOWS.md#L15) |

## What the notes found

The older unified browser-shell studies separate page services from the host's surface lifetime. The current engine-owned guest contract supplies that distinction directly. [inferred]
- 2026-11-08: A webview given a window id shows an engine-owned browser page that survives shell reloads with scroll, form and history state; a webview with a src belongs to the shell document and ends with it; a second webview on the same window id stays empty [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/webview-element.ts#L71>
- 2026-11-08: The engine build is full Chrome with Views widgets in use, but the shell SDK exposes DOM tags and desktop operations rather than Views control constructors; the tab-helper header skips browser UI helpers because the shell draws that UI [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/chrome/browser/domicile/domicile_tab_helpers.h>
- 2026-11-06: A Chrome 156 --app window on a plain http page reported the window identity chrome-127.0.0.1__-Default with the port omitted; two http app windows on one origin therefore share one identity. [measured]
- 2026-11-06: A Chrome --app window opened on a distinct local start page kept a path-derived identity after navigating to http; two such windows stayed distinct within one Chrome process. [measured]
- 2026-11-06: Chrome app windows used server-side decorations; screenshots showed no address strip and no second title bar beneath the shell title bar. [measured]
- 2026-11-07: A shell-side placement path for an app window can carry a fullscreen request's selected display through compositor and SDK; a Manganese revision that dropped it returned a second-display window to the primary display after leaving fullscreen [measured]
- 2026-11-09: Highlighting a launcher bookmark loads its signed-in page; icon fetching uses no cookies, and selecting the bookmark opens a browser window in the shell (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/LAUNCHER.md#L81> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/LAUNCHER.md#L103>
- 2026-11-07: Opening an HTTP or HTTPS address bypasses the configured MIME browser handler and uses a desktop browser window (Domicile@pin) [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-launch/src/open.rs#L133-L147>
- 2026-11-09: The engine retains a desk-wide download list beyond an individual browser window; Manganese exposes progress, cancel, open, reveal and list-removal actions from that list (Domicile@996d8b628439a5d9b12204b49c9d472f249666bc) [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/shell-manganese/docs/TOP-BAR.md#L440>
- 2026-11-04: The shell page runs under --app in Domicile's patched Chromium; `<app>` and `<webview>` are engine-native tags, not registered web components, so stock Chrome app mode does not provide the shell contract [docs] <https://github.com/cprussin/domicile/blob/08f0687dafa1018a9dcd7f227f43cc6dc557ffec/docs/WRITING-A-SHELL.md#what-a-shell-is>
- 2026-11-05: The open command with --app and --id reuses one engine-owned app window by id and returns after opening; the command's lifetime is not the window's lifetime [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WEB-APPS.md#L27>
- 2026-11-08: A separate native application built on Views could be embedded through the app element without engine changes, as an inferred route. It stays a native surface: styling reaches its outer surface only, and shared state needs IPC [inferred] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/docs/ELEMENTS.md#L9>
- 2026-11-06: Meta-key shell shortcuts kept reaching the shell while a Chrome app window had focus on Domicile [docs]
- 2026-11-04: browser.openApp(url, {id, reuse}) gives named app-window reuse; workspace placement stays shell policy, and windows report id, url and appId [docs]

## Where it touches Domicile

- `engine:browser`: owns matching and browser pages; `sdk`: exposes app-window operations; `manganese`: handles app-window behavior. [App identity and reuse](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WEB-APPS.md#L22). [docs]
- `session`: the launcher installs entries from web manifests and dispatches app-window requests through the supervisor. [Install contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WEB-APPS.md#L47). [docs]
- Status at the pinned checkout: `landed`, the merged app-window [initial implementation #1125](https://github.com/cprussin/domicile/pull/1125), [origin behavior #1179](https://github.com/cprussin/domicile/pull/1179), and [reuse #1692](https://github.com/cprussin/domicile/pull/1692) are reflected in the pinned design. [App-window completion](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/WEB-APPS.md#L124). [docs]

- App-origin checking [compares scheme and host](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/browser/app-origin.ts#L9); unparseable addresses count as outside the app origin. The initial reference is [stored when the shell component mounts](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/src/BrowserWindow.tsx#L275), while current page identity remains engine-owned. [source] A surviving guest does not by itself preserve a remounted component's local origin reference. [inferred]

- Removing the embedding webview from a [connected parent](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_web_view_element.cc#L343) resets its host, guest and client Mojo endpoints. [Insertion requests guest binding](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_web_view_element.cc#L331) again; the browser-window attribute skips ordinary src navigation. [source] These hooks describe element bindings alongside the documented engine-owned page lifetime; they supply no additional browser-page survival result. [inferred]

## Lineage

- COWL and CUBS examined a browser process shared with the shell; their [unified-binary chapter](../../01-chromium-lineage/mechanisms/unified-binary.md) keeps that historical assumption sealed.

## Sources

- S-5c4621c3f33a · 2026-11-09 · Pinned web app and app-window contract
- S-0aa579827d73 · 2026-11-09 · Pinned browser-window lifetime
- S-9852d15a44e1 · 2026-11-09 · Domicile app-origin comparison semantics
- S-14b41dc319ce · 2026-11-09 · Domicile browser-window focus, origin and motion handling
- S-fcddb6547d9d · 2026-11-09 · Domicile webview element binding, detach and navigation hooks
