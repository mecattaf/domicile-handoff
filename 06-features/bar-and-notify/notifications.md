---
id: bar-and-notify/notifications
title: Notifications, history and actions
kind: feature
family: bar-and-notify
layers: [compositor, host, sdk, manganese]
domicile_status: landed
domicile_refs: [cprussin/domicile#1876, cprussin/domicile#1885, docs/architecture/NOTIFICATIONS.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [kde, noctalia]
eras: []
rows: [SW-0127, SW-0298, SW-0299, SW-0300, SW-0301, SW-0302, SW-0303, SW-0304, SW-0305, SW-0306, SW-0307, SW-0308, SW-0309, SW-0310, SW-0311, SW-0312, SW-0313, SW-0314, SW-0315, SW-0316, SW-0317, SW-0318, SW-0319, SW-0320, SW-0321, SW-0322, SW-0323, SW-0324, SW-0325, SW-0326, SW-0327, SW-0328, SW-0329, SW-0330, SW-0331, SW-0332, SW-0333, SW-0334, SW-0335, SW-0336, SW-0337, SW-0338, SW-0339, SW-0340, SW-0341, SW-0342, SW-0343, SW-0344, SW-0345, SW-0346, SW-0347, SW-0348, SC-0205, SC-0206, SC-0207, SC-0208, SW-0674, SW-0675, SW-0676, SW-0677, SW-0678, SW-0679, SW-0680, SW-0681, SW-0682, SW-0683, SW-0684, SW-0685, SW-0686, SW-0687, SW-0688, SW-0689, SW-0690, SW-0691, SW-0692, SW-0693, SW-0694, SW-0695, SW-0696, SW-0697, SW-0698, SW-0699, SW-0700, SW-0701, SW-0702, SW-0703, SW-0704, SW-0705, SW-0706, SW-0707, SW-0708, SW-0709, SW-0710, SW-0711, SW-0712, SW-0713, SW-0714, SW-0715, SW-0716, SW-0717, SC-0371, SC-0372, SC-0373, SC-0374, SC-0375, SC-0376, SC-0377]
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: false
bucket: domicile
tags: [family:bar-and-notify, layer:compositor, status:landed, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-0c4d3fd4676a, S-1ebe9cf0b16d, S-3b9bcd430d39, S-695f43c6be84, S-7b433be8e99b, S-830c16d0468b, S-8b26af7753e9, S-9a57cc2646c0, S-9bade2fc7ff6, S-a6a7fef88d3a, S-ad57df68c679, S-b3963c8c1a30, S-b4d50969eebc, S-b9e10b3eb20e, S-cd7659cd27df, S-d7864d339412, S-e193e1cee954, S-e68d7932aa6c, S-e7f93d14c1ff, S-ed234a9db7b5]
---
# Notifications, history and actions

Notifications remain available in history after their toast expires, and actions return to the application that sent them.

## Behavior

- Toast expiry and clearing history are distinct operations.
- Actions and inline replies carry user interaction back to the sender.
- Do not disturb suppresses ordinary toasts while retaining incoming notifications; critical items bypass suppression.
- Notification history can outlive a shell-page reload.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| KDE Plasma @ pinned source | History and actionable-state model | Expiry retains the history item but clears actions; explicit closure queues removal, and sender disappearance clears remaining actions. | [History closure](https://github.com/KDE/plasma-workspace/blob/b0f268a13a0860aeda5f8dbeeb075058358fadb3/libnotificationmanager/abstractnotificationsmodel.cpp#L174) · [Sender lifetime](https://github.com/KDE/plasma-workspace/blob/b0f268a13a0860aeda5f8dbeeb075058358fadb3/libnotificationmanager/abstractnotificationsmodel.cpp#L57) |
| Noctalia @ v5.4.1 | Separate persisted history and session action lookup | When current-session notifications arrive before history loads, restored identifiers are reassigned before current history is merged; actions target active or session-pending-close entries. [docs] | [Release history and action handling](https://github.com/noctalia-dev/noctalia/blob/v5.4.1/src/notification/notification_manager.cpp#L942) · [Release history and action handling](https://github.com/noctalia-dev/noctalia/blob/v5.4.1/src/notification/notification_manager.cpp#L516) |

## What the notes found

- Checked 2026-11-09: The attached SW/SC rows describe Swaynag and Scrollnag helper command contracts. The desktop notification service and its history are separate contracts. [source] [Helper interface](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/swaynag/swaynag.1.scd#L1)
- Checked 2026-11-09: Reload persistence is scoped: notification history lives in host state, while the do-not-disturb switch is retained by Manganese. That is distinct from retaining the entire window layout. [source] [History](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/NOTIFICATIONS.md#L63) · [Do not disturb](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/NOTIFICATIONS.md#L130)
- Checked 2026-11-09: The Ash survey separates a desktop notification server from Chromium’s Linux notification client; sharing message-center concepts does not supply the session D-Bus service. [docs] [Message center build](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/ui/message_center/BUILD.gn#L1) · [Linux bridge](https://github.com/chromium/chromium/blob/9261fd0a595ac4964ea84e6bd4a025c1173a2ffa/chrome/browser/notifications/notification_platform_bridge_linux.cc#L1)
- Checked 2026-11-09: A numeric notification value hint is absent from the host’s retained hint fields; built-in volume, microphone and brightness meters take a separate level-notice path. [source] [Retained hints](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/notifications.rs#L86) · [Level notice](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/shell-manganese/src/LevelNotice.tsx#L19)
- Checked 2026-11-09: KDE separates replacement from a new arrival: refreshed contents and timeout retain the item’s creation, read, dismissal and expiry state. Action lifetime is separate from history lifetime. [source] [Replacement](https://github.com/KDE/plasma-workspace/blob/b0f268a13a0860aeda5f8dbeeb075058358fadb3/libnotificationmanager/abstractnotificationsmodel.cpp#L144)
- Checked 2026-11-09: KDE also separates numeric level notices from its notification manager through a dedicated OSD service object. [source] [Numeric notice service](https://github.com/KDE/plasma-workspace/blob/b0f268a13a0860aeda5f8dbeeb075058358fadb3/shell/osd.cpp#L39)
- 2026-10-01: The Ash system census distinguishes the cross-platform message-center model and Views from Chrome’s Linux notification client. Running a desktop notification service is a separate D-Bus role (Ash survey) [docs] <https://github.com/chromium/chromium/blob/150.0.7871.124/ui/message_center/BUILD.gn> <https://github.com/chromium/chromium/blob/150.0.7871.124/chrome/browser/notifications/notification_platform_bridge_linux.cc>
- 2026-10-26: The notes distinguish libnotify session-bus delivery, browser notifications and terminal notification escapes; Wayland belongs to the presenter, while a remote byte stream does not automatically choose the local notification service (freedesktop notifications) [docs] <https://specifications.freedesktop.org/notification/latest-single/>
- 2026-06-11: The June machinery audit distinguishes Chromium’s freedesktop notification client bridge from a desktop shell owning the notification server; MessageCenter supplies a store and callbacks rather than that server role (Chromium historical studies) [docs]
- 2026-10-02: The notification study records popup gaps after restacking: widget bounds resize a layer surface while unchanged protocol margins retain its old placement. Painting a reusable notification view does not complete native placement (views-shell@October-2026) [docs]
- 2026-03-27: The March service audit treats notifications as a D-Bus server with identifier allocation, image decoding and capability negotiation, rather than a thin proxy for an existing notification daemon (QuickshellX historical service audit) [docs]
- 2026-11-04: The November audit separates persisted notification data from callable actions; restored history does not guarantee that the original application still accepts an action (noctalia@2026-11-survey) [docs]
- 2026-11-04: A November study records separate DMS limits for visible notifications, queued entries and incoming arrivals, with distinct handling for critical notifications (dms@2026-11-survey) [docs]
- 2026-06-30: A historical shell synthesis separates do-not-disturb popup suppression from notification storage; notifications can still enter history while visual interruptions are suppressed (Historical shell service synthesis) [docs]
- 2026-11-05: The [desktop-events contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-DESKTOP-EVENTS.md#L133) orders the uncleared notification list oldest first and treats the inline-reply action key as a reply carrying text. [docs]

## Where it touches Domicile

- `compositor`: the notification D-Bus frontend owns the service name, methods and signals. [source] [Backend split](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/NOTIFICATIONS.md#L21)
- `host`: the host library maps incoming notification data, retains history and validates action keys. [source] [Retained state](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/notifications.rs#L129) · [Action validation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/notifications.rs#L217)
- `sdk`: the notification namespace exposes the current list, changes, invoke, reply and dismiss. [source] [Notification namespace](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-DESKTOP-EVENTS.md#L133)
- `manganese`: toasts, history and do-not-disturb behavior use that public service. [source] [History and suppression](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/NOTIFICATIONS.md#L63)
- Status: `landed`; notification work in [#1876](https://github.com/cprussin/domicile/pull/1876) and [#1885](https://github.com/cprussin/domicile/pull/1885) is reflected in the pin’s architecture and API docs. [source]

## Lineage

- 2026-11-09: the pin documents actions, inline replies and history independently of toast lifetime. [source] [Action semantics](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/NOTIFICATIONS.md#L83)

## Sources

- S-9a57cc2646c0 · 2026-11-09 · [Notification backend and retained history](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/NOTIFICATIONS.md#L1).
- S-a6a7fef88d3a · 2026-11-09 · [Typed desktop event namespaces](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-DESKTOP-EVENTS.md#L1).
- S-3b9bcd430d39 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-0c4d3fd4676a · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-b3963c8c1a30 · 2026-11-09 · [Notification history and action validation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-host/src/notifications.rs#L1).
- S-e193e1cee954 · 2026-11-09 · [Numeric device-level notice behavior](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/shell-manganese/src/LevelNotice.tsx#L1).
- S-ad57df68c679 · 2026-11-08 · KDE notification model at the donor pin.
- S-e7f93d14c1ff · 2026-11-08 · KDE notification model at the donor pin.
- S-8b26af7753e9 · 2026-11-08 · KDE notification model at the donor pin.
- S-cd7659cd27df · 2026-10-29 · Noctalia release source and state contracts.
