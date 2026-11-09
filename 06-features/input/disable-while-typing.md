---
id: input/disable-while-typing
title: Disable touchpads while typing
kind: feature
family: input
layers: [engine:input, config, session, external]
domicile_status: landed
domicile_refs: [docs/RUNNING-A-DESKTOP.md, cprussin/domicile#1884]
checked_at: 996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09
answered_upstream: false
scroll_parity: baseline
donors: [sway, scroll, mutter]
rows: []
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [family:input, layer:engine:input, status:landed, donor:sway, donor:scroll, donor:mutter, evidence:source, scroll-parity:baseline]
sources: [S-1b21754fae3d, S-1be0caf83f39, S-2d0a5d452253, S-2e9434cd1711, S-3335cd2786c6, S-4819d73df19e, S-500fe6fd17da, S-912ac2de77f7, S-99698015bc48, S-9bce4d22631c, S-a732558c81f0, S-baadedc7c85c, S-bafc4a630b63, S-c92aa706df61, S-d70587a15fa9, S-e21a73fec98d, S-e92a22a88dc7, S-f6401f8c13cc]
---
# Disable touchpads while typing

Typing suppression has two distinct policies at the pin: the engine can pause touchpad events with its own guard, or delegate suppression to libinput. The custom guard and native libinput policy have different timing and contact rules. [Policy selection](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/input_settings.cc#L178) [source]

## Behavior

- The custom policy defaults to enabled, with a 200 ms pause after one typing key and 500 ms during continued typing. Both pauses accept zero. The desktop documentation states that any keyboard participates. [Desktop contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/RUNNING-A-DESKTOP.md#L161) [docs] · [Defaults](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/input_settings.h#L38) · [Pause validation](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/input_settings.cc#L71) [source]
- Ctrl, Alt and Meta combinations do not start the custom pause. Shift combined with a typing key does. Typing counts as continuing while a suppression pause remains active or when the preceding typing press was within 200 ms. The configured pause durations remain separate. [Typing recognition](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/typing_guard.cc#L15) [source]
- A drag whose button press passed through keeps motion flowing on that device. A suppressed button press also suppresses its matching release; button identities include the device. [Motion and button gates](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/typing_guard.cc#L82) [source]
- The internal-only policy selects internal touchpads for the custom guard. Native libinput mode bypasses that guard, enables native DWT only when supported and enabled, and rejects custom pause values. [Pad eligibility](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/input_settings.cc#L178) · [Native setter](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0175-domicile-share-libinput-devices-and-native-typing.patch#L640) [source]

## How donors do it

| Donor at pin | Mechanism | Observable behavior or limit | Link |
|---|---|---|---|
| sway | Applies native libinput DWT configuration | Capability and unchanged-value checks guard the setter; defaults use a separate reset path | [Native settings](https://github.com/swaywm/sway/blob/1652c54b73f67df17b7b4ab0b0f7048204aa8104/sway/input/libinput.c#L201) |
| scroll | Retains the libinput settings path | Native DWT remains a device setting, separate from layout gestures | [Native settings](https://github.com/dawsers/scroll/blob/b6586d273fed7d187744b7eaec2c234386a029fa/sway/input/libinput.c#L201) |
| GNOME Mutter | Checks native DWT availability before applying it | A separate setter forwards timeout milliseconds to libinput | [DWT gate](https://github.com/GNOME/mutter/blob/b766717ac9ad04d44211557ec79a545dc8c44ee7/src/backends/native/meta-input-settings-native.c#L259) · [Timeout setter](https://github.com/GNOME/mutter/blob/b766717ac9ad04d44211557ec79a545dc8c44ee7/src/backends/native/meta-input-settings-native.c#L278) |

## What the notes found

- Native suppression cannot be reduced to one timeout. As a fixed library reference, libinput 1.31.3 can release a suppressed contact after timeout when it began after the latest key press. A contact held through additional typing follows a different path. This reference does not establish the library version or physical behavior of the Domicile build. [Contact classification](https://gitlab.freedesktop.org/libinput/libinput/-/blob/1.31.3/src/evdev-mt-touchpad.c#L832) [source]
- In that library reference, internal pads pair with internal-tagged keyboards. An external pad pairs only with a keyboard sharing its vendor and product identifiers, and native DWT availability additionally requires the below-keyboard combo quirk. These are native library rules, separate from the custom guard's any-keyboard contract. [Keyboard pairing](https://gitlab.freedesktop.org/libinput/libinput/-/blob/1.31.3/src/evdev-mt-touchpad.c#L2291) · [External-pad availability](https://gitlab.freedesktop.org/libinput/libinput/-/blob/1.31.3/src/evdev-mt-touchpad.c#L3351) [source]
- Sharing a native input context permits native keyboard and touchpad processing together. It does not establish which physical keyboards pair with a particular pad or when a held contact resumes on hardware. [Shared context](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/libinput_seat.cc#L23) [source] Physical pairing and contact acceptance remain unqualified at this pin. [inferred]
- 2026-11-09: The desktop docs describe a touchpad disable-while-typing guard on by default: a key pauses the pads for 200 ms, continued typing for 500 ms, both configurable including zero, and any keyboard counts [source] <https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/RUNNING-A-DESKTOP.md#L161>
- 2026-11-05: libinput lets a contact that begins after the last key press move again once the timeout expires. Disposable-VM typing-policy checks passed on a contact held between two typing keys, across startup, hotplug and reload [measured] <https://wayland.freedesktop.org/libinput/doc/1.26.0/palm-detection.html>
- 2026-11-05: libinput's native disable-while-typing needs keyboard and touchpad in one libinput context. At a 2026-11-05 snapshot each pad had its own context, so native suppression could not see keyboard activity [docs] <https://wayland.freedesktop.org/libinput/doc/1.26.0/palm-detection.html>
- 2026-11-05: A shared-seat backend lets libinput pair an internal pad with its internal keyboard and keep physical clicks usable. Config offers this native policy or Domicile's guard with adjustable pauses, and rejects custom pause durations in native mode [docs]

## Where it touches Domicile

- `engine:input` owns typing recognition, pause state, per-device button bookkeeping and native libinput configuration. The patch “domicile: share libinput devices and native typing”, carried by [#1884](https://github.com/cprussin/domicile/pull/1884), supplies the native path. [Guard](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/typing_guard.cc#L64) · [Native mode](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/patches/0175-domicile-share-libinput-devices-and-native-typing.patch#L640) [source]
- `config` selects enabled, disabled, internal-only or native-libinput behavior and validates nonnegative pauses. The broader [touchpad and mouse configuration](touchpad-and-mouse-config.md) page covers the remaining device controls. [Policy fields](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/input_settings.cc#L133) [source]
- `session` supplies already-authorized device descriptors to the shared libinput context; the input callback duplicates those descriptors. [Authorized device path](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/libinput_seat.cc#L37) [source]
- Status at the pin: landed. [#1884](https://github.com/cprussin/domicile/pull/1884) carries the shared native libinput path and configurable custom guard; the [pinned typing contract](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/RUNNING-A-DESKTOP.md#L161) describes those policies. Physical pairing and contact acceptance remain unqualified. [docs] [Pinned policy](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/input_settings.cc#L178) [source]

## Lineage

- 2026-11-06: [Native and custom typing policies](https://github.com/cprussin/domicile/pull/1884) merge upstream. [docs]

- 2026-11-09: the pinned source separates the custom guard from native libinput mode. [Eligibility and timing](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-engine/src/ui/events/ozone/evdev/domicile/input_settings.cc#L178) [source]

## Sources

- S-a732558c81f0 · 2026-11-06 · Shared native input and custom typing-policy merge record.

- S-f6401f8c13cc · 2026-11-09 · Pinned desktop input and startup contract.

- S-9bce4d22631c · 2026-11-09 · Documentary desktop input-policy record.
- S-4819d73df19e · 2026-11-09 · Custom typing recognition and device button lifetime.
- S-500fe6fd17da · 2026-11-09 · Typing policy selection and pause validation.
- S-99698015bc48 · 2026-11-09 · Default typing policy and pause values.
- S-e21a73fec98d · 2026-11-09 · Shared authorized native input context.
- S-baadedc7c85c · 2026-11-09 · Native input setting application.
- S-e92a22a88dc7 · 2026-11-09 · Shared native input and typing policy routing.
- S-1be0caf83f39 · 2026-11-09 · Sway native input setting application.
- S-2d0a5d452253 · 2026-11-09 · Scroll native input setting application.
- S-c92aa706df61 · 2026-11-09 · Mutter native suppression and timeout setters.
- S-912ac2de77f7 · 2026-11-09 · Libinput 1.31.3 contact-suppression reference.
