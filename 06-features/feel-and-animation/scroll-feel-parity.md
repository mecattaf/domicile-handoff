---
id: feel-and-animation/scroll-feel-parity
title: Scroll feel and animation continuity
kind: feature
family: feel-and-animation
layers: [engine:input, engine:blink, manganese]
domicile_status: landed-with-gap
domicile_refs: [cprussin/domicile#1943, cprussin/domicile#1910, cprussin/domicile#1939, cprussin/domicile#1942, docs/KNOWN-GAPS.md]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: true
scroll_parity: baseline
donors: [scroll, kde, niri, swayfx]
eras: []
rows: [SW-0130, SW-0131, SW-0144, SW-0169, SW-0185, SW-0192, SW-0193, SW-0194, SW-0195, SW-0196, SW-0197, SW-0198, SW-0199, SW-0200, SW-0201, SW-0210, SW-0212, SW-0213, SW-0215, SW-0216, SW-0220, SW-0221, SW-0235, SW-0247, SC-0013, SC-0014, SC-0015, SC-0016, SC-0017, SC-0018, SC-0019, SC-0020, SC-0021, SC-0022, SC-0023, SC-0024, SC-0025, SC-0026, SC-0027, SC-0035, SC-0036, SC-0037, SC-0038, SC-0052, SC-0053, SC-0054, SC-0055, SC-0056, SC-0064, SC-0066, SC-0094, SC-0095, SC-0096, SC-0097, SC-0098, SC-0099, SC-0100, SC-0101, SC-0102, SC-0106, SC-0107, SW-0434, SW-0435, SW-0436, SW-0437, SW-0448, SW-0449, SW-0450, SW-0451, SW-0452, SW-0453, SW-0454, SW-0455, SW-0456, SW-0457, SW-0518, SW-0519, SW-0520, SW-0577, SW-0578, SW-0579, SW-0580, SW-0581, SW-0582, SW-0596, SW-0597, SW-0598, SW-0599, SW-0600, SW-0601, SW-0602, SW-0603, SW-0604, SW-0605, SW-0606, SW-0607, SW-0608, SW-0609, SW-0610, SW-0611, SW-0612, SW-0613, SW-0614, SW-0615, SW-0616, SW-0617, SW-0631, SW-0632, SW-0633, SW-0658, SW-0659, SW-0660, SW-0661, SW-0662, SW-0663, SW-0664, SW-0665, SC-0216, SC-0217, SC-0340, SC-0341, SC-0342, SC-0343, SC-0344, SC-0345, SC-0346, SC-0347, SC-0359, SC-0360, SC-0361, SC-0362, SC-0456, SC-0457, SC-0458, SC-0459, SC-0461, SC-0463, SC-0464, SC-0465, SC-0466, SC-0467, SC-0468, SC-0469, SC-0491, SC-0507, SC-0508, SC-0509, SC-0510, SC-0511, SC-0512, SC-0514, SC-0522, SC-0523, SC-0527, SC-0552, SC-0553, SC-0554, SC-0555, SC-0556, SC-0557, SC-0558, SC-0559, SC-0560, SC-0561, SC-0562, SC-0563, SC-0564, SC-0565, SC-0566, SW-1056, SW-1058, SW-1059, SW-1060, SW-1061, SW-1105, SW-1106, SW-1107, SW-1108, SW-1109, SW-1110, SW-1111, SW-1112, SC-0641, SC-0642, SW-1291, SW-1292, SW-1293, SW-1294, SC-0682, SC-0683, SC-0684, SC-0685, SC-0701]
depends_on: []
evidence_mix: mostly-source
reported_on_domicile: []
boundary_note: false
bucket: domicile
tags: [family:feel-and-animation, layer:engine:input, status:landed-with-gap, evidence:source, scroll-parity:baseline, bucket:domicile]
sources: [S-1193ff48f66d, S-204a969601d0, S-23c521755f22, S-2b285c11dec3, S-4a9f3490b0b6, S-4f20a12f585a, S-60d9036b7ed9, S-65d1c77e9380, S-6bec7a90d975, S-7cb0007051a1, S-8100b35ed50a, S-83dc7cc92be3, S-8620cc6bd9c2, S-92faa6ff2e46, S-a92c840fc324, S-b1a9c0db3aa5, S-c84e70d4b8e8, S-de92af3f5a9b, S-fd0c87388c02]
---
# Scroll feel and animation continuity

Scroll feel depends on event phases, target selection, direct-manipulation behavior and animation interruption, as well as the visible easing curve.

## Behavior

- Slow updates and zero-speed endings remain observable sequences.
- A fling reaches the same target that received the preceding scroll.
- Direct manipulation follows gesture deltas rather than waiting for ordinary placement animation.
- Interrupting a transition retargets from the current displayed value.

## How donors do it

| Donor @ pin | Mechanism | Behavior and limits | Link |
|---|---|---|---|
| scroll @ pinned source | Gesture-dependent viewport movement | Primary-axis movement affects the workspace; cross-axis movement affects the pointed group, which can differ from keyboard focus. | [Gesture routing](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3383) |
| scroll @ pinned source | Animated variable state | Affected variables retarget from their displayed value while unrelated curves continue. | [Animation state](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/include/sway/desktop/animation.h#L53) |
| KWin @ pinned source | Retained closing windows and spring retargeting | An effect guard retains closed-window state; retargeting a spring changes its anchor while retaining position and velocity. | [Lifetime guard](https://github.com/KDE/kwin/blob/0406762df7794213f12ce87bc8f6fd322dcdc53c/src/effect/effectwindow.h#L841) · [Spring target](https://github.com/KDE/kwin/blob/0406762df7794213f12ce87bc8f6fd322dcdc53c/src/effect/springmotion.cpp#L118) |
| niri @ pinned source | Velocity-bearing springs and shared timing | Swipe-release velocity enters spring motion; some resize-driven motion shares timing, with documented unsynchronized paths. | [Spring velocity](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Configuration:-Animations.md#L110) · [Resize timing](https://github.com/YaLTeR/niri/blob/90d5c1bf7381a81c953cb8b3f26c5ddcaa3f48cb/docs/wiki/Configuration:-Animations.md#L457) |
| Swayfx @ 0.6 | Interactive arrangement bypass and retargeting | Interactive resize and floating drag bypass arrangement animation; other moves retarget from tracked current geometry and opacity. [docs] | [Release transaction policy](https://github.com/wlrfx/swayfx/blob/0.6/sway/desktop/transaction.c#L730) |

## What the notes found

- Checked 2026-11-09: Scroll suspends ordinary placement animation during direct-manipulation updates and restores it at the end; sensitivity and workspace scale affect its deltas. [source] [Direct manipulation](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3340) · [Delta scaling](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/tree/layout.c#L3389)
- 2026-11-04: scroll exposes animations as config and runtime commands, with global enable, per-event curves (window size, fullscreen, overview, layer surfaces) and N-order Bezier control points (scroll@b6586d2) [source] <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L68> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L148> <https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/scroll.5.scd#L433>
- 2026-11-07: Native browser history navigation remained unexplained in a 2026-11-07 input report; normal-scale synthetic controls navigated, and no physical or final-pin reproduction was established [docs]
- 2026-11-07: Scroll's continuous gesture deltas are scaled by sensitivity and layout scale before reaching the layout (scroll@b6586d2) [docs]

## Where it touches Domicile

- `engine:input`: The page-fling correction emits fling start before native axis stop and emits a zero-velocity fling boundary on a slow release. [source] [Patch: domicile: a flick's glide starts before the scroll stop](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0190-domicile-a-flick-s-glide-starts-before-the-scroll-stop.patch#L39) · [#1943](https://github.com/cprussin/domicile/pull/1943)
- `engine:blink`: The native-window element discards synthetic momentum wheels while retaining native source and axis-stop data. [source] [Native momentum boundary](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/third_party/blink/renderer/core/html/domicile/html_app_element.cc#L451)
- `manganese`: Closing tabs retain and shrink their contents; arriving workspace elements are marked resizing so the slide can use their latest available frames. [source] [#1910](https://github.com/cprussin/domicile/pull/1910) · [#1939](https://github.com/cprussin/domicile/pull/1939) · [#1942](https://github.com/cprussin/domicile/pull/1942)
- Status: `landed-with-gap` for those event-order and animation-continuity slices; physical presentation timing remains unmeasured in the public gap list. Exact Scroll timing, viewport policy and donor option parity are separate rows in the [feel family](../../04-sway-family/scroll-parity/animations-and-feel.md). [source] [Pin animation contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L400)
- Native input and momentum have their own [input contract page](../../06-features/input/scroll-feel-and-momentum.md). The presentation limit is recorded at the pin. [source] [Known presentation gap](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md#L30)

## Lineage

- 2026-11-08: page glide and slow-scroll sequence loss were corrected in the engine. [source] [#1943](https://github.com/cprussin/domicile/pull/1943)

## Sources

- S-204a969601d0 · 2026-11-06 · sway and Scroll semantic inventory.
- S-23c521755f22 · 2026-11-09 · [Grouped layouts, workspace homes and interaction](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/manganese-window-management/docs/WINDOW-MANAGEMENT.md#L1).
- S-fd0c87388c02 · 2026-11-08 · Public Domicile change catalog.
- S-8100b35ed50a · 2026-11-09 · [Documented desktop boundaries at the pin](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/KNOWN-GAPS.md#L1).
- S-7cb0007051a1 · 2026-11-08 · KWin animation and lifetime source at the donor pin.
- S-4a9f3490b0b6 · 2026-11-08 · KWin animation and lifetime source at the donor pin.
- S-6bec7a90d975 · 2026-11-08 · KWin animation and lifetime source at the donor pin.
- S-4f20a12f585a · 2026-11-08 · Niri animation contract at the donor revision.
- S-de92af3f5a9b · 2026-11-09 · Swayfx release documentation.
- S-2b285c11dec3 · 2026-11-09 · pinned desktop contract and donor source comparison.
- S-a92c840fc324 · 2026-11-09 · pinned desktop contract and donor source comparison.
