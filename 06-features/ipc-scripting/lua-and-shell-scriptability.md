---
id: ipc-scripting/lua-and-shell-scriptability
title: Lua semantics and reusable shell modules
kind: feature
family: ipc-scripting
layers: [any-shell, sdk, manganese]
domicile_status: by-design-elsewhere
domicile_refs: [docs/WRITING-A-SHELL.md, docs/SHELL-FROM-MANGANESE.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, miracle-wm, pinnacle]
eras: []
rows: [SW-0091, SW-0112, SW-0113, SW-0114, SW-0115, SW-0116, SW-0117, SW-0118, SW-0119, SW-0120, SW-0121, SW-0123, SW-0124, SW-0125, SW-0129, SW-0132, SW-0149, SW-0150, SW-0151, SW-0168, SW-0170, SW-0178, SW-0202, SW-0203, SW-0211, SW-0231, SW-0233, SW-0234, SW-0272, SW-0274, SW-0275, SW-0276, SW-0277, SW-0278, SW-0279, SW-0280, SW-0282, SW-0284, SW-0285, SW-0286, SW-0287, SW-0288, SW-0291, SW-0292, SW-0293, SW-0294, SW-0295, SW-0296, SW-0297, SC-0028, SC-0029, SC-0117, SC-0118, SC-0119, SC-0120, SC-0121, SC-0122, SC-0123, SC-0124, SC-0125, SC-0126, SC-0127, SC-0128, SC-0129, SC-0130, SC-0131, SC-0132, SC-0133, SC-0134, SC-0135, SC-0136, SC-0137, SC-0138, SC-0139, SC-0140, SC-0141, SC-0142, SC-0143, SC-0144, SC-0145, SC-0146, SC-0147, SC-0148, SC-0149, SC-0150, SC-0151, SC-0152, SC-0153, SC-0154, SC-0155, SC-0156, SC-0157, SC-0158, SC-0159, SC-0160, SC-0161, SC-0162, SC-0163, SC-0164, SC-0165, SC-0166, SC-0167, SC-0168, SC-0169, SC-0170, SC-0171, SC-0172, SC-0173, SC-0174, SC-0175, SC-0176, SC-0177, SC-0178, SC-0179, SC-0180, SC-0181, SC-0182, SC-0183, SC-0184, SC-0185, SC-0186, SC-0187, SC-0188, SC-0189, SC-0190, SC-0191, SC-0192, SC-0193, SC-0194, SC-0195, SC-0196, SC-0197, SC-0198, SC-0199, SC-0200, SC-0202, SC-0204, SW-0418, SW-0419, SW-0420, SW-0421, SW-0422, SW-0423, SW-0424, SW-0431, SW-0432, SW-0433, SW-0666, SW-0668, SW-0669, SW-0670, SW-0671, SW-0672, SW-0673, SC-0370, SW-1018, SW-1026, SW-1028, SW-1029, SW-1031, SC-0549, SC-0550, SC-0551, SW-1034, SW-1035, SW-1036, SW-1037, SW-1039, SW-1040, SW-1052, SW-1073, SW-1076, SW-1086, SW-1087, SW-1100, SW-1101, SW-1103, SC-0567, SC-0568, SC-0569, SC-0570, SC-0571, SC-0572, SC-0573, SC-0574, SC-0575, SC-0576, SC-0577, SC-0578, SC-0579, SC-0580, SC-0581, SC-0582, SC-0583, SC-0584, SC-0585, SC-0586, SC-0587, SC-0588, SC-0589, SC-0590, SC-0591, SC-0592, SC-0593, SC-0594, SC-0595, SC-0596, SC-0597, SC-0598, SC-0599, SC-0600, SC-0601, SC-0602, SC-0603, SC-0604, SC-0605, SC-0606, SC-0607, SW-1121, SW-1122, SW-1123, SW-1124, SW-1125, SW-1126, SW-1127, SW-1128, SW-1129, SW-1130, SW-1131, SW-1132, SW-1133, SW-1134, SW-1135, SW-1136, SW-1137, SC-0608, SW-1138, SW-1139, SW-1140, SW-1141, SC-0609, SW-1147, SW-1207, SW-1208, SW-1220, SW-1222, SC-0648, SC-0649, SC-0650, SC-0651, SC-0652, SC-0653, SC-0654, SC-0655, SC-0656, SC-0657, SC-0658, SC-0659, SC-0661, SC-0664, SC-0666, SC-0667, SC-0668, SC-0669, SC-0670, SC-0671]
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: false
bucket: domicile
tags: [family:ipc-scripting, layer:any-shell, status:by-design-elsewhere, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-0deb267198f4, S-204a969601d0, S-32b5544fb772, S-333f76cea05b, S-3c7d077c30fb, S-541270a86f10, S-6b79c52fc5cd, S-862b24b56e86, S-97b683c7bb7f, S-9bce4d22631c, S-b5101babba40, S-c0a5420d3a23, S-d5b15decb594, S-e77ffff40807]
---
# Lua semantics and reusable shell modules

Scroll embeds Lua for layout operations and persistent script state; Domicile exposes a JavaScript shell contract and independently reusable Manganese packages.

## Behavior

- A script can inspect layout state, calculate a placement and invoke desktop commands.
- Script-local retained state and desktop-wide state have separate lifetimes.
- A reusable module can export behavior without reproducing a donor’s interpreter or command parser.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| scroll @ pinned source | Lua file and inline evaluation | Named inline programs retain isolated local state; helpers inspect windows, workspaces, outputs and virtual outputs. | [Invocation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1938) · [State helpers](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L2201) |
| sway @ pinned source | External programs over IPC | The tree read model and commands compose outside the compositor process. | [Socket request families](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-ipc.7.scd#L34) |
| miracle-wm @ v0.11.2 | Ordered WebAssembly policy modules | New-window placement goes to modules in order until one handles it. [docs] | [Release reference](https://github.com/miracle-wm-org/miracle-wm/blob/v0.11.2/wiki/docs/configuration/plugins.md#L4) |
| Pinnacle @ v0.2.4 | External asynchronous configuration program | Rust configuration connects as a compositor client over the public gRPC socket. [docs] | [Release reference](https://github.com/pinnacle-comp/pinnacle/blob/v0.2.4/api/rust/src/lib.rs#L34) · [Release reference](https://github.com/pinnacle-comp/pinnacle/blob/v0.2.4/api/rust/src/lib.rs#L98) |

## What the notes found

- Checked 2026-11-09: The Scroll API supports retained script state, JSON conversion, command dispatch and named IPC events, alongside window and output inspection. These are separate semantic surfaces. [source] [Lua helpers](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L2190)
- Checked 2026-11-09: Published examples use Lua to coordinate linked windows, swallowing, workspace initialization, selection moves and remembered maximize geometry. [docs] [Placement examples](https://github.com/dawsers/scroll/issues/35#issuecomment-3049737169) · [Swallowing example](https://github.com/dawsers/scroll/issues/35#issuecomment-3066728670)
- Checked 2026-11-09: The map callback runs after criteria and before focus selection; initial fullscreen handling and arrangement have already run. The command-end callback covers Lua-executed commands rather than every command in the compositor. [source] [Mapping order](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1063) · [Command callback](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L507)
- Checked 2026-11-09: The configuration owns one Lua interpreter, with retained storage tables for individual scripts. Separate storage does not imply a separate interpreter for each script. [source] [Lua command state](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/lua.c#L7) · [Configuration lifetime](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config.c#L306)
- 2026-11-04: scroll documents a Lua scripting surface in the same man page, including functions such as animations_get_enabled and animations_set_enabled (scroll@b6586d2) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L2219-L2222>
- 2026-11-09: A terminal command, send-shell, runs a shell command as a keybinding bound to it would, for example focusing right, giving scripts a path into the shell's command table [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/RUNNING-A-DESKTOP.md#L129>

## Where it touches Domicile

- `any-shell`: `Shell(root, domicile)` is a framework-independent JavaScript entry point; a WebAssembly shell can bind the same interface. [source] [Shell entry](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L34) · [WebAssembly](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L214)
- `manganese`: configuration, custom bar items and mounting independent packages are documented public routes. [source] [Reusable packages](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md#L71)
- `sdk`: typed desktop state and events are available to those modules. [source] [SDK contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L237)
- Status: `by-design-elsewhere`; module composition and the shell API provide the public scripting route. An embedded Scroll-compatible Lua runtime is not addressed in the pin’s command surface. [source] [Shell modules](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md#L71) · [Command inventory](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L38)

## Lineage

- 2026-11-05: the pinned Scroll manual includes Lua invocation, retained script state and desktop inspection helpers. [source] [Lua](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L1938)

## Sources

- S-204a969601d0 · 2026-11-06 · sway and Scroll semantic inventory.
- S-32b5544fb772 · 2026-09-21 · [Desktop request and event protocol](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/sway-ipc.7.scd#L1).
- S-e77ffff40807 · 2026-11-09 · [Shell entry, client elements and native-window control](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/WRITING-A-SHELL.md#L1).
- S-541270a86f10 · 2026-11-09 · [Reusable shell modules and public exports](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-FROM-MANGANESE.md#L1).
- S-d5b15decb594 · 2026-11-09 · [Window command vocabulary and default bindings](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/KEYS.md#L1).
- S-862b24b56e86 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-6b79c52fc5cd · 2026-11-05 · [Native view lifecycle and matching rules](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/view.c#L1).
- S-333f76cea05b · 2026-11-05 · [Lua accessors, callbacks and interoperability](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/lua.c#L1).
- S-b5101babba40 · 2026-11-05 · [Lua invocation and explicit command context](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/commands/lua.c#L1).
- S-c0a5420d3a23 · 2026-11-05 · [Compositor configuration and interpreter lifetime](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/config.c#L1).
- S-0deb267198f4 · 2026-09-26 · miracle-wm release documentation.
- S-3c7d077c30fb · 2026-06-27 · Pinnacle release API documentation.
