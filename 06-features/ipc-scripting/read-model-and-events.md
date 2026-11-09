---
id: ipc-scripting/read-model-and-events
title: Desktop state and change events
kind: feature
family: ipc-scripting
layers: [sdk, host, protocol, manganese]
domicile_status: by-design-elsewhere
domicile_refs: [docs/WRITING-A-SHELL.md, docs/architecture/KEYBINDINGS.md, docs/CLI.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, niri, miracle-wm]
eras: []
rows: [SW-0718, SW-0719, SW-0720, SW-0721, SW-0722, SW-0723, SW-0724, SW-0725, SW-0726, SW-0727, SW-0728, SW-0729, SW-0730, SW-0731, SW-0732, SW-0733, SW-0734, SW-0735, SW-0736, SW-0737, SW-0738, SW-0739, SW-0740, SW-0741, SW-0742, SW-0743, SW-0744, SW-0745, SW-0746, SW-0747, SW-0748, SW-0749, SW-0750, SW-0751, SW-0752, SW-0753, SW-0754, SW-0755, SW-0756, SW-0757, SW-0758, SW-0759, SW-0760, SW-0761, SW-0762, SW-0763, SW-0764, SW-0765, SW-0766, SW-0767, SW-0768, SW-0769, SW-0770, SW-0771, SW-0772, SW-0773, SW-0774, SW-0775, SW-0776, SW-0777, SW-0778, SW-0779, SW-0780, SW-0781, SW-0782, SW-0783, SW-0784, SW-0785, SW-0786, SW-0787, SW-0788, SW-0789, SW-0790, SW-0791, SW-0792, SW-0793, SW-0794, SW-0795, SW-0796, SW-0797, SW-0798, SW-0799, SW-0800, SW-0801, SW-0802, SW-0803, SW-0804, SW-0805, SW-0806, SW-0807, SW-0808, SW-0809, SW-0810, SW-0811, SW-0812, SW-0813, SW-0814, SW-0815, SW-0816, SW-0817, SW-0818, SW-0819, SW-0820, SW-0821, SW-0822, SW-0823, SW-0824, SW-0825, SW-0826, SW-0827, SW-0828, SW-0829, SW-0830, SW-0831, SW-0832, SW-0833, SW-0834, SW-0835, SW-0836, SW-0837, SW-0838, SW-0839, SW-0840, SW-0841, SW-0842, SW-0843, SW-0844, SW-0845, SW-0846, SW-0847, SW-0848, SW-0849, SW-0850, SW-0851, SW-0852, SW-0853, SW-0854, SW-0855, SW-0856, SW-0857, SW-0858, SW-0859, SW-0860, SW-0861, SW-0862, SW-0863, SW-0864, SW-0865, SW-0866, SW-0867, SW-0868, SW-0869, SW-0870, SW-0871, SW-0872, SW-0873, SW-0874, SW-0875, SW-0876, SW-0877, SW-0878, SW-0879, SW-0880, SW-0881, SW-0882, SW-0883, SW-0884, SW-0885, SW-0886, SW-0887, SW-0888, SW-0889, SW-0890, SW-0891, SW-0892, SW-0893, SW-0894, SW-0895, SW-0896, SW-0897, SW-0898, SW-0899, SW-0900, SW-0901, SW-0902, SW-0903, SW-0904, SW-0905, SW-0906, SW-0907, SW-0908, SW-0909, SW-0910, SW-0911, SW-0912, SW-0913, SW-0914, SW-0915, SW-0916, SW-0917, SW-0918, SW-0919, SW-0920, SW-0921, SW-0922, SW-0923, SW-0924, SW-0925, SC-0378, SC-0379, SC-0380, SC-0381, SC-0382, SC-0383, SC-0384, SC-0385, SC-0386, SC-0387, SC-0388, SC-0389, SC-0390, SC-0391, SC-0392, SC-0393, SC-0394, SC-0395, SC-0396, SC-0397, SC-0398, SC-0399, SC-0400, SC-0401, SC-0402, SC-0403, SC-0404, SC-0405, SC-0406, SC-0407, SC-0408, SC-0409, SC-0410, SC-0411, SC-0412, SC-0413, SC-0414, SC-0415, SC-0416, SC-0417, SC-0418, SC-0419, SC-0420, SC-0421, SC-0422, SC-0423, SC-0424, SC-0425, SC-0426, SC-0427, SC-0428, SC-0429, SC-0430, SC-0431, SC-0432, SC-0433, SC-0434, SC-0435, SC-0436, SC-0437, SC-0438, SC-0439, SC-0440, SC-0441, SC-0442, SC-0443, SC-0444, SC-0445, SC-0446, SC-0447, SC-0448, SC-0449, SC-0450, SC-0451, SC-0452, SC-0453, SC-0454, SC-0455, SW-0957, SW-0958, SW-0959, SW-0960, SW-0961, SW-0962, SW-1149, SW-1150, SW-1151, SW-1152, SW-1153, SW-1154, SW-1155, SW-1156, SW-1157, SW-1158, SW-1159, SW-1160, SW-1161, SW-1162, SW-1163, SW-1164, SW-1165, SW-1166, SW-1167, SW-1168, SW-1169, SW-1170, SW-1171, SW-1172, SW-1173, SW-1174, SW-1175, SW-1176, SW-1177, SW-1178, SW-1179, SW-1180, SW-1181, SW-1182, SW-1183, SW-1184, SW-1185, SW-1186, SW-1187, SW-1188, SW-1189, SW-1190, SW-1191, SW-1192, SW-1193, SW-1194, SW-1195, SW-1196, SW-1197, SW-1198, SW-1199, SW-1200, SW-1201, SW-1202, SC-0619, SC-0620, SC-0621, SC-0622, SC-0623, SC-0624, SC-0625, SC-0626, SC-0627, SC-0628, SC-0629, SC-0630, SC-0631, SC-0632, SC-0633, SC-0634, SC-0635, SC-0636, SC-0637, SC-0638, SC-0639, SC-0640, SW-1203, SW-1204, SW-1205, SW-1206]
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: true
bucket: domicile
tags: [family:ipc-scripting, layer:sdk, status:by-design-elsewhere, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-00f885e06232, S-10cfd46e0cfd, S-1fa1acbff487, S-201202a3c112, S-204a969601d0, S-278d2a39212b, S-32b5544fb772, S-33784ae23b70, S-3533e3f0274a, S-382a33e43f29, S-39c541407074, S-537297eb1290, S-591141579c64, S-5b008b0584e5, S-6ca6958d79ba, S-782e09e7d435, S-a331aea31e68, S-a70560fdc087, S-b3bcc537fcfa, S-c40e302fa0da, S-c6f3a38054b8, S-ced00bd7e275, S-e77ffff40807]
---
# Desktop state and change events

The public shell API exposes typed desktop state and change events; terminal commands enter the shell’s own command handler.

## Behavior

- A consumer reads current state before listening for later changes.
- Window identity, focus, displays, notifications and tray state are separate namespaces.
- Commands report whether a shell is listening and are refused while locked.
- Wire compatibility and equivalent observable state are separate forms of parity.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| sway @ pinned source | i3-compatible socket requests and subscriptions | Tree, workspaces, outputs, marks, bindings, inputs and seats have separate request types. | [Request types](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-ipc.7.scd#L34) |
| scroll @ pinned source | Inherited socket plus layout and Lua events | Lua code can emit JSON data as a named event. | [Lua event](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L2208) |
| niri @ pinned source | Initial state followed by change events | The state stream reduces polling, but cross-object updates can expose a temporary reference to a removed workspace. | [State stream](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/IPC.md#L11) |
| miracle-wm @ v0.11.2 | Workspace events with placement policy | Tile/float policy is reported separately from tree layout, focus and output moves. [docs] | [Release reference](https://github.com/miracle-wm-org/miracle-wm/blob/v0.11.2/wiki/docs/ipc/events/workspace.md#L14) |

## What the notes found

- Checked 2026-11-09: The donor map separates a transport’s request and payload vocabulary from the smaller set of reusable desktop-state contracts. The [IPC family](../../04-sway-family/scroll-parity/ipc.md) contains the row accounting. [docs]
- Checked 2026-11-09: Scroll’s scroller event carries a change string and a full workspace-scroller snapshot. Its content rectangle is surface-relative geometry; Domicile’s boolean client-frame preference is a different field contract. [source] [Event snapshot](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/ipc-server.c#L470) · [Scroller record](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/ipc-json.c#L1572) · [Content rectangle](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/ipc-json.c#L623) · [Native-frame flag](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1190)
- 2026-11-04: sway-ipc defines message types RUN_COMMAND, GET_WORKSPACES, SUBSCRIBE, GET_OUTPUTS, GET_TREE, GET_MARKS, GET_BAR_CONFIG, GET_VERSION, GET_BINDING_MODES, GET_CONFIG, SEND_TICK, SYNC, GET_BINDING_STATE, GET_INPUTS and GET_SEATS, plus a set of subscribable events (sway@1652c54) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-ipc.7.scd#L88> <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-ipc.7.scd#L1473>
- 2026-11-05: sway IPC defines eight subscribable event types (workspace, output, mode, window, barconfig_update, binding, shutdown, tick), the tick event also acting as a sync marker after SEND_TICK. [source] (sway@1652c54) [source] <https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-ipc.7.scd#L1473-L1515>
- 2026-10-02: A 2026-10-02 adapter study separates accepted requests from observed state: it subscribes before querying, rebuilds snapshots after reconnect, and waits for an observed echo before changing its model (views@2026-10-02) [docs]
- 2026-06-30: A 2026-06-30 external compositor-client study updates internal state on every event while coalescing page-facing broadcasts and retains the ordered event stream [docs]
- 2026-11-04: At the 2026-11-04 snapshot the domicile CLI verbs sit below React and any script can call them, with send-shell as one route into the page [docs]
- 2026-11-06: A review of 343 IPC contracts, fields and events noted that detailed display modes, per-device XKB state and per-window idle inhibitor state were not exposed to the page [docs]
- 2026-11-07: On a 2026-11-07 snapshot the SDK exposes each native window's kernel-recorded process identity and PID incarnation, which identifies a window's client process without title heuristics [docs]
- 2026-11-05: The scroll IPC manual documents a window node with fully_visible, shell (xdg_shell or xwayland), inhibit_idle, idle_inhibitors, sandbox identity, xdg toplevel tag, and XWayland window properties; workspace and output nodes carry rect, scale, transform and mode data (scroll@b6586d273fed7d187744b7eaec2c234386a029fa) [docs]
- 2026-06-12: miracle-wm reimplemented the i3 IPC protocol on Mir; a June 2026 study cited this as evidence that the i3 IPC vocabulary can be served by a non-wlroots compositor (miracle-wm) [docs]

## Where it touches Domicile

- `sdk`: typed namespaces expose attributes and listeners; window state includes focus and change events. [source] [Read and listen](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L138) · [Window namespace](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/windows.ts#L18)
- `host`: The host-library shell-command fan-out tells every listening page and returns success, or an error when no page listens. [source] [Fan-out owner](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/KEYBINDINGS.md#L65)
- `protocol`: Terminal shell commands travel as `system_request send_shell` and `system_event shell_command` messages. [source] [Command wire route](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/KEYBINDINGS.md#L57)
- `manganese`: its typed command vocabulary interprets the received arguments. [source] [Shell commands](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/CLI.md#L81)
- Status: `by-design-elsewhere`; the documented interfaces are shell namespaces and shell commands, rather than sway socket framing. [source] [SDK contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L237)

## Open boundary

Open boundary: programmatic control of the desktop (reading state, typed actions, event streams) is outside this library. The closest donor surface is sway IPC `get_tree`.

## Lineage

- 2026-11-05 donor revision: Scroll exposes Lua event emission alongside its inherited desktop socket vocabulary. [source] [Lua event emission](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L2208)

## Sources

- S-32b5544fb772 · 2026-09-21 · [Desktop request and event protocol](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-ipc.7.scd#L1).
- S-204a969601d0 · 2026-11-06 · sway and Scroll semantic inventory.
- S-1fa1acbff487 · 2026-11-09 · [Native window identity, bounds and focus namespace](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/desktop/windows.ts#L1).
- S-e77ffff40807 · 2026-11-09 · [Shell entry, client elements and native-window control](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L1).
- S-537297eb1290 · 2026-11-09 · [Key tables, grabs and shell-command routing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/KEYBINDINGS.md#L1).
- S-782e09e7d435 · 2026-11-09 · [Shell loading and command dispatch](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/CLI.md#L1).
- S-33784ae23b70 · 2026-11-08 · Niri state-stream contract at the donor revision.
- S-382a33e43f29 · 2026-09-26 · miracle-wm release documentation.
- S-278d2a39212b · 2026-11-05 · [Desktop subscriptions and scroller events](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/ipc-server.c#L1).
- S-10cfd46e0cfd · 2026-11-05 · [Desktop state serialization and content geometry](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/ipc-json.c#L1).
- S-a70560fdc087 · 2026-11-09 · [Typed native-window and desktop metadata](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/chrome-sdk/src/domicile-host.ts#L1).
