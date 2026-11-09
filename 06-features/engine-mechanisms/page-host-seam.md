---
id: engine-mechanisms/page-host-seam
title: Typed page and host calls
kind: feature
family: engine-mechanisms
layers: [engine:browser, sdk, host, compositor]
domicile_status: unknown
checked_at: "2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc"
answered_upstream: false
scroll_parity: n/a
donor_availability: rule-portable
donors: []
evidence_mix: mixed
boundary_note: false
bucket: domicile
tags: [family:engine-mechanisms, evidence:source, bucket:domicile]
sources: [S-1cd4484a3a81, S-2fcac3a67284, S-39c541407074, S-4a11b3f74e93, S-612a99e2b687, S-9d1497c20da4, S-9eeaa5387ca1, S-b3ee402f8bb3, S-c40e302fa0da, S-c6f3a38054b8, S-dfd7b6e6962d, S-e22876f2be14, S-e77ffff40807, S-ef77e5cd4455, S-fcd733bf5e55]
eras: [COWL revived, CUBS]
rows: [AL-cade-triage-017, AL-cade-triage-018, AL-cade-triage-021, AL-cade-triage-115, AL-cade-decision-004, AL-cade-decision-005, AL-cade-decision-008, AL-cade-fourth-verdicts-002, AL-desktop-service-transport-003, AL-desktop-service-transport-010, AL-desktop-service-transport-018]
depends_on: []
reported_on_domicile: []
domicile_refs: [docs/WRITING-A-SHELL.md]
---
# Typed page and host calls

A page makes a typed host call while native presentation and window policy retain their own contracts. Call identity, interface registration and service lifetime are independent parts of the seam. [source] ([Typed call transport](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/system-calls.ts#L1))

## Behavior

- Call identifiers correlate replies and event streams with the initiating operation. [source] ([Transport contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/system-calls.ts#L5))
- A service receiver binding is explicit. The further distinction between a typed interface and its authorization policy is a semantic inference. [inferred] ([Binding interface](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ui/webui/mojo_web_ui_controller.h#L28))

## How donors do it

| Donor @ pin | Mechanism | Notable behavior or limit | Link |
|---|---|---|---|
| Chromium WebUI @ 150 | Each Mojo service has an explicit receiver-binding entry | A controller overload and binder registration connect a typed interface to its browser implementation | [Binding contract](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ui/webui/mojo_web_ui_controller.h#L28) |
| Domicile @ desktop pin | SDK calls carry identifiers and receive correlated system events | Recoverable subscriptions re-register after the compositor returns and emit a missed-events indication | [Restart handling](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/system-calls.ts#L635) |

The WebUI header is an interface-pattern reference; its presence does not establish that this controller is linked into Domicile's trimmed engine. [inferred]

## What the notes found

- A 2026-06-16 COWL study separated typed service calls from Wayland pixel submission and compositor window/workspace IPC. It reported moving a service lifetime to the frame host. [docs] S-dfd7b6e6962d.
- 2026-11-07: The engine can expose read-only shell scale, shell presentation transforms and embedded guest scale through the DevTools protocol, so scale state can be read in a test rather than estimated visually [measured]
- 2026-11-04: A new method on the page-side domicile object counts as an engine change, because that object is compiled into Chromium, even when the wire is an opaque relay; the notes separate page, compositor and engine as three rebuild cuts [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-SYSTEM-ACCESS.md>
- 2026-11-04: The shell page is served from the domicile scheme with no loopback port; browser.fetch is HTTPS-only with profile cookies and does not forward custom origins [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/DOMICILE-SCHEME.md>
- 2026-11-04: The shell hands the engine a native window SurfaceId rather than a JavaScript texture; capture.preview() returns PNG data URLs, and no zero-copy texture access to `<app>` content is documented, so shader sampling of native windows is unproven through the shell API [docs] <https://github.com/cprussin/domicile/blob/08f0687dafa1018a9dcd7f227f43cc6dc557ffec/docs/SHELL-SYSTEM-ACCESS.md#page-api>
- 2026-11-04: Settings pages reach the engine through a stdio app host, then DOMICILE_SOCK to the domicile command socket, then the engine; engine verbs include settings_files, site_permissions, set_site_permission, media_devices and set_media_device [docs] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SETTINGS.md>
- 2026-11-04: The shell page runs scripts only from its own local root; loading shell plugins from a remote host is not provided as of this snapshot [docs]
- 2026-11-08: The engine routes trusted input from the page through native app elements toward native windows; the SDK exposes real native-window embedding to the shell page, while an ordinary browser window of another web app does not receive that shell authority [docs]

## Where it touches Domicile

- The pinned [shell-writing contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md) is Domicile’s public page/host interface. The historical namespace does not add another API requirement. [docs]
- The SDK [system-call implementation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/system-calls.ts#L635) treats compositor restart as a transport event with explicit subscription recovery. [source]

- The pixel-delivery broker [waits for the requested app’s sink](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/frame_sink_broker.cc#L94), rejects embed tokens retained from disconnected producers, and [checks producer ownership](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/components/domicile/browser/frame_sink_broker.cc#L191) before buffer import, submission, fencing or destruction. Typed service calls and native frame delivery therefore retain distinct lifetimes and authorization surfaces. [source]

Era and its assumptions: COWL and CUBS, which assumed a separate window manager beneath the shell. For Domicile this probably adds little: Domicile already supplies a typed SDK and a page/host contract. Recorded because service lifetime and presentation transport remain separate concerns. [docs]

## Lineage

- 2026-06-16: the COWL revival separated service/data transport, pixel delivery and external compositor policy. [docs]
- 2026-06-29: the CUBS study retained Mojo for typed services while Views owned rendering. [docs] S-9eeaa5387ca1.

## Sources

- S-b3ee402f8bb3 · 2026-11-09 · Chromium typed WebUI receiver binding interface
- S-dfd7b6e6962d · 2026-06-16 · COWL service lifetime and transport separation study
- S-612a99e2b687 · 2026-11-09 · Domicile correlated system-call and recovery implementation
- S-9eeaa5387ca1 · 2026-06-29 · CUBS typed-service and rendering separation
- S-ef77e5cd4455 · 2026-11-09 · Domicile producer ownership, pending embeds and disconnect handling
- S-e77ffff40807 · 2026-11-09 pin check · Pinned shell authoring contract
