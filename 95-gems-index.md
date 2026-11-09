---
id: 95-gems-index
title: Isolated findings from material otherwise outside the map
kind: collection
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
bucket: domicile
evidence_mix: mixed
tags: [bucket:domicile]
sources: []
---

# Isolated findings from material otherwise outside the map

Single facts from studies that are otherwise outside the map, grouped by the feature page they bear on. Each line carries its source id and date; treat a line as `[docs]` unless its page says otherwise.

## keyboard-and-text/text-input-and-ime

- Client xkb compose processing is distinct from compositor-side input-method/text-input factories (S-4aaee696f4de · 2026-07-18)

## layout/scrolling-layout

- Niri keeps per-monitor window strips and independent vertical workspaces; disconnected workspaces can return to the original monitor (S-23423b4671c9 · 2026-11-08)
- Overview retains all keyboard shortcuts and supports pointer/touch moving and workspace dragging (S-a3bd9859358a · 2026-11-08)
- Each monitor owns a dynamic workspace list with bottom empty workspace and remembered connector home (S-48603351a6ee · 2026-11-08)
- Tabbed display is a column presentation; navigation uses same per-column window operations (S-0d7150dbb6e9 · 2026-11-08)
- State stream sends initial state and changes; cross-object updates are not always atomic (S-33784ae23b70 · 2026-11-08)

## outputs/output-management-and-power

- Pinned display documentation defines profiles through connector enablement, placement, scale and transform. Its mode entry checks pixel dimensions; connectors still take native modes, and the profile cannot choose refresh rate (S-d0a4ca3453bd · 2026-11-09)
- Wayland idle inhibitors hold only for live surfaces present in the displayed-window set. Portal/application holds are stored separately, and the same combined veto controls both blanking and automatic locking (S-d0a4ca3453bd · 2026-11-09)
- EIS input keeps per-client pointer position and held buttons, then translates movement and buttons into ordinary client requests. That route shares the native-client lock gate (S-d0a4ca3453bd · 2026-11-09)
- After hotplug, a blanked desktop explicitly restates disabled connectors. The hotplug guard acts only while idle blanking is dark; it is separate from the compositor lock state (S-d0a4ca3453bd · 2026-11-09)
- The lock gate permits KeyboardFocus while suppressing client input. Its documented purpose is agreement between page focus and compositor focus after unlock (S-d0a4ca3453bd · 2026-11-09)
- A newly connected shell page receives the compositor’s current lock state. Unlock verification broadcasts the resulting state to every connected page (S-d0a4ca3453bd · 2026-11-09)

## terminal/kitty-over-ssh-and-latency

- Wayland compositor frame callback and presentation-feedback arrival occur after terminal byte input (S-93bfe16ec4f6 · 2026-07-18)

## terminal/kitty-wayland-protocols

- Client buffer scaling, terminal geometry and presentation callbacks are separate from byte-arrival timing (S-93bfe16ec4f6 · 2026-07-18)
- Default middle-button release invokes paste_from_selection; mapping belongs to Kitty client configuration (S-ab93695e401f · 2026-07-18)
- Kitty is an ordinary native Wayland client; shell policy remains separate (S-42ecb235c7f2 · 2026-10-26)
- Ordinary Wayland terminal surfaces and browser-embedded terminals have different ownership boundaries (S-c3ab182267b1 · 2026-11-03)

## workspaces/overview

- Ash BUILD target asserts is_chromeos (S-a486a03ed3e3 · 2026-11-09)

## workspaces/workspace-model-and-homes

- The COWL workspace reference associates workspace groups with outputs and documents an initial group and workspace snapshot when subscriptions start (S-d2b0492d5f4d · 2026-11-09)
- The June dock draft identifies separate running-window and application-pin producers; it records unresolved sibling-widget dependencies and competing work-area writers, so the drafted shelf reuse is not a completed implementation (S-d2b0492d5f4d · 2026-11-09)
- A 2026-06-30 external compositor-client study updates internal state on every event while coalescing page-facing broadcasts and retains the ordered event stream (S-d2b0492d5f4d · 2026-11-09)
- Ash’s `TrayNetworkStateModel` binds a CrosNetworkConfig remote and observer receiver; its direct observer callbacks are a separate boundary from its Mojo service channel (S-d2b0492d5f4d · 2026-11-09)
- WindowTargeter sends key events to the FocusClient window. If EventClient denies processing within that subtree, it clears focus and returns no key target (S-d2b0492d5f4d · 2026-11-09)
- Ash uses one active desk container identifier on all display roots. Activation and deactivation update every associated container together, while per-root maps preserve container and stacking bookkeeping; the donor remains ChromeOS-gated (S-d2b0492d5f4d · 2026-11-09)
- Ash stores relative stacking positions for windows visible on all desks per display root and restores those positions among ordinary windows. Floated windows use separate association and stacking paths (S-d2b0492d5f4d · 2026-11-09)

