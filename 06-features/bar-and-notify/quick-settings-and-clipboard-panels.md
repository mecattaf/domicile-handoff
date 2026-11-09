---
id: bar-and-notify/quick-settings-and-clipboard-panels
title: Quick settings and clipboard panels
kind: feature
family: bar-and-notify
layers: [manganese, any-shell, sdk, external]
domicile_status: landed
domicile_refs: [packages/manganese-quick-settings/README.md, packages/system-bluetooth/src/bluetooth.ts, packages/system-network/src/networkmanager-wifi.ts, cprussin/domicile#1612, cprussin/domicile#1551]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: n/a
donors: []
eras: []
rows: []
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: [L-17, L-18]
boundary_note: false
bucket: domicile
tags: [family:bar-and-notify, layer:manganese, status:landed, evidence:source, bucket:domicile]
sources: [S-16bf250a5f9c, S-19f56e984ec5, S-1a653077d862, S-39879e79c09f, S-5a9e3b32e108, S-749b21ddc68b, S-7ff35abbccb9, S-b530093fc8bc, S-c97aba8aa41d, S-f3f39588bcf4]
---
# Quick settings and clipboard panels

Quick settings exposes system controls through reusable shell packages; the clipboard panel displays compositor-held history.

## Behavior

- Network and Bluetooth panels use the available system-service backend.
- New credential entry and activation of an existing connection are distinct operations.
- Pairing without a code and pairing that requires confirmation have different service requirements.
- Clipboard-panel state can be reconstructed from service-held entries after a page reload.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| Domicile @ pinned source | Independent quick-setting and clipboard packages | The shell panel and the service owning its state have separate lifetimes. | [Quick settings](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-quick-settings/README.md#L3) · [Clipboard reconstruction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/src/useClipboard.ts#L6) |

## What the notes found

- Checked 2026-11-09: Bluetooth pairing accepts devices that do not require a code; the source records no pairing callback service for PIN, passkey or confirmation flows. [source] [Pairing limit](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-bluetooth/src/bluetooth.ts#L125)
- Checked 2026-11-09: The NetworkManager backend creates new secured profiles with WPA-PSK credentials. Existing profiles use a separate activation path, so this does not describe every enterprise connection configured elsewhere. [source] [Existing profiles](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-network/src/networkmanager-wifi.ts#L89) · [New profiles](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-network/src/networkmanager-wifi.ts#L146)
- Checked 2026-11-09: The IWD backend separately uses its bus API for known or open networks and invokes its command client for a new secured network. [source] [IWD path](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-network/src/iwd-wifi.ts#L77)
- 2026-11-08: The Manganese clipboard history keeps 32 entries bounded to 128 MiB in memory only, with text search and image thumbnails; file drags and primary selections are excluded, and nothing is written to disk [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/clipboard.rs#L14> <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/docs/CLIPBOARD.md>
- 2026-11-08: Pairing sets Trusted before connecting. [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-bluetooth/src/bluetooth.ts#L126>
- 2026-11-08: Enterprise credentials, hidden SSID entry and saved-network deletion are not addressed as of the pin. [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-network/src/networkmanager-wifi.ts#L158>

## Where it touches Domicile

- `manganese`: quick settings combines network, Bluetooth, audio, brightness, battery and idle controls; independent bar items use the same packages. The clipboard panel reads its initial entry snapshot and watches changes while mounted. [source] [Quick-settings package](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-quick-settings/README.md#L3) · [Clipboard consumer lifetime](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/src/useClipboard.ts#L21)
- `any-shell`: reusable web-side network and Bluetooth packages implement the connection and pairing paths, with the authentication and pairing bounds recorded above. [source] [System-service adapters](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ARCHITECTURE.md#L159) · [Pairing path](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-bluetooth/src/bluetooth.ts#L125) · [New network profiles](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-network/src/networkmanager-wifi.ts#L146)
- `sdk`: the network and Bluetooth adapters use SDK D-Bus calls. The clipboard namespace supplies pushed history through `entries` and `change` events. [source] [Adapter call surface](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ARCHITECTURE.md#L159) · [Clipboard namespace use](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/src/useClipboard.ts#L6)
- `external`: the network and Bluetooth adapters contact NetworkManager, iwd or wpa_supplicant for networking, and BlueZ for Bluetooth. [source] [System-service peers](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ARCHITECTURE.md#L159)
- Status: `landed`; quick-settings package integration [#1612](https://github.com/cprussin/domicile/pull/1612) and the typed API port used by the clipboard package [#1551](https://github.com/cprussin/domicile/pull/1551) are merged before the pin. The pairing and new-credential limits above are bounded source findings at the pin. The clipboard protocol contract is [clipboard ownership](../../06-features/clipboard-dnd/data-control-and-primary-selection.md). [source] [Current quick settings](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-quick-settings/README.md#L3)

## Lineage

- 2026-10-27: quick settings moved into an independent package used by the default bar. [source] [#1612](https://github.com/cprussin/domicile/pull/1612)
- 2026-11-09: pinned source distinguishes panel lifetime, service-held state and backend-specific credential paths. [source] [Clipboard replay](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/src/useClipboard.ts#L6)

## Sources

- S-c97aba8aa41d · 2026-11-09 · [Reusable system-panel package contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-quick-settings/README.md#L1).
- S-749b21ddc68b · 2026-11-09 · [Bluetooth connection and pairing boundaries](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-bluetooth/src/bluetooth.ts#L1).
- S-16bf250a5f9c · 2026-11-09 · [Network profile activation and credential paths](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-network/src/networkmanager-wifi.ts#L1).
- S-7ff35abbccb9 · 2026-11-09 · [Network service connection and credential handling](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/system-network/src/iwd-wifi.ts#L1).
- S-19f56e984ec5 · 2026-11-09 · [Clipboard snapshots and change subscriptions](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-clipboard/src/useClipboard.ts#L1).
- S-f3f39588bcf4 · 2026-10-27 · Public merge metadata for quick-settings package integration.
- S-39879e79c09f · 2026-10-27 · Default bar import integration in the merged package change.
- S-b530093fc8bc · 2026-10-25 · Public merge metadata for the typed desktop API port.
- S-1a653077d862 · 2026-11-09 · [Shell, SDK and system-service boundaries](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ARCHITECTURE.md#L1).
