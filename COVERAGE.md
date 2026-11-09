# Coverage

One row per feature page. Sorted by family then id; no ordering beyond that.

| id | layers | status at pin | donors | scroll parity | evidence | reported symptoms |
|---|---|---|---|---|---|---|
| [aura-wm/desks-and-virtual-desktops](06-features/aura-wm/desks-and-virtual-desktops.md) | manganese, sdk, compositor | unknown | ash | n/a | mixed | 0 |
| [aura-wm/display-management](06-features/aura-wm/display-management.md) | engine:drm, compositor, config | unknown | ash | n/a | mixed | 0 |
| [aura-wm/overview-mode](06-features/aura-wm/overview-mode.md) | engine:blink, sdk, manganese | landed | ash | n/a | mixed | 0 |
| [aura-wm/split-view-and-cycle](06-features/aura-wm/split-view-and-cycle.md) | manganese, sdk, compositor | unknown | ash | n/a | mixed | 0 |
| [aura-wm/window-states](06-features/aura-wm/window-states.md) | manganese, sdk, compositor | unknown | ash, exo | n/a | mixed | 0 |
| [bar-and-notify/notifications](06-features/bar-and-notify/notifications.md) | compositor, host, sdk, manganese | landed | kde, noctalia | baseline | mostly-source | 0 |
| [bar-and-notify/quick-settings-and-clipboard-panels](06-features/bar-and-notify/quick-settings-and-clipboard-panels.md) | manganese, any-shell, sdk, external | landed |  | n/a | mostly-source | 2 |
| [bar-and-notify/status-bar-and-tray](06-features/bar-and-notify/status-bar-and-tray.md) | manganese, host, sdk, compositor, system-apps | landed | sway, scroll | baseline | mostly-source | 1 |
| [bindings-and-modes/binding-modes](06-features/bindings-and-modes/binding-modes.md) | sdk, engine:browser, compositor, manganese | landed | sway, scroll, jay | baseline | mostly-source | 0 |
| [browser-windows/chrome-dialogs-in-shell](06-features/browser-windows/chrome-dialogs-in-shell.md) | engine:browser, sdk, manganese | landed-with-gap | views | n/a | mostly-source | 0 |
| [browser-windows/tab-groups-and-strips](06-features/browser-windows/tab-groups-and-strips.md) | engine:browser, sdk, manganese | unknown | views | n/a | mixed | 0 |
| [browser-windows/web-apps-and-app-windows](06-features/browser-windows/web-apps-and-app-windows.md) | engine:browser, sdk, manganese, session | landed |  | n/a | mostly-source | 0 |
| [build-and-packaging/nix-and-engine-builds](06-features/build-and-packaging/nix-and-engine-builds.md) | session, config | landed |  | n/a | mixed | 0 |
| [capture/foreign-toplevel-and-taskbar](06-features/capture/foreign-toplevel-and-taskbar.md) | compositor, protocol, sdk, any-shell | landed-with-gap | sway, scroll, exo | baseline | mostly-source | 0 |
| [capture/screencopy-and-image-capture](06-features/capture/screencopy-and-image-capture.md) | engine:viz, compositor, protocol, sdk, any-shell, external | landed | sway, scroll, smithay | baseline | mostly-source | 0 |
| [clipboard-dnd/data-control-and-primary-selection](06-features/clipboard-dnd/data-control-and-primary-selection.md) | engine:browser, compositor, protocol, sdk, manganese | landed-with-gap | sway, scroll, exo, smithay | baseline | mostly-source | 0 |
| [effects/backdrop-blur](06-features/effects/backdrop-blur.md) | engine:blink, engine:viz, any-shell | known-gap | aura, ash, kitty | n/a | mostly-source | 0 |
| [effects/corner-radius-shadows](06-features/effects/corner-radius-shadows.md) | engine:blink, engine:viz, manganese | unknown | aura, ash, swayfx, exo | beyond | mixed | 0 |
| [effects/window-animations](06-features/effects/window-animations.md) | engine:blink, engine:viz, manganese | landed | aura, ash, kde, niri, swayfx, exo | n/a | mostly-source | 0 |
| [engine-mechanisms/engine-fork-and-patch-discipline](06-features/engine-mechanisms/engine-fork-and-patch-discipline.md) | engine:browser, session | landed |  | n/a | mixed | 0 |
| [engine-mechanisms/gpu-and-render-backends](06-features/engine-mechanisms/gpu-and-render-backends.md) | engine:browser, engine:viz, engine:drm, compositor | landed-with-gap | aura, exo | n/a | mixed | 1 |
| [engine-mechanisms/page-host-seam](06-features/engine-mechanisms/page-host-seam.md) | engine:browser, sdk, host, compositor | unknown |  | n/a | mixed | 0 |
| [feel-and-animation/scroll-feel-parity](06-features/feel-and-animation/scroll-feel-parity.md) | engine:input, engine:blink, manganese | landed-with-gap | scroll, kde, niri, swayfx | baseline | mostly-source | 0 |
| [feel-and-animation/workspace-slide-bounce](06-features/feel-and-animation/workspace-slide-bounce.md) | manganese, engine:blink | landed | scroll | baseline | mostly-source | 0 |
| [gestures/gesture-claim-api](06-features/gestures/gesture-claim-api.md) | engine:input, engine:browser, sdk, manganese | landed-with-gap | sway, scroll, niri, kde, ash, fusuma, libinput-gestures | baseline | mostly-source | 0 |
| [gestures/multi-finger-swipe-and-hold](06-features/gestures/multi-finger-swipe-and-hold.md) | engine:input, engine:browser, compositor, protocol, sdk, manganese | landed-with-gap | sway, scroll, niri, kde, ash, fusuma, libinput-gestures | baseline | mostly-source | 0 |
| [gestures/touchpad-pinch](06-features/gestures/touchpad-pinch.md) | engine:input, engine:blink, engine:browser, compositor, protocol | landed | sway, niri, smithay, libinput-gestures | baseline | mostly-source | 0 |
| [gestures/touchscreen-and-edge](06-features/gestures/touchscreen-and-edge.md) | engine:input, engine:blink, engine:browser, compositor, protocol, sdk, manganese | landed | smithay, lisgd, fusuma | baseline | mostly-source | 0 |
| [input/disable-while-typing](06-features/input/disable-while-typing.md) | engine:input, config, session, external | landed | sway, scroll, mutter | baseline | mostly-source | 0 |
| [input/scroll-feel-and-momentum](06-features/input/scroll-feel-and-momentum.md) | engine:input, engine:blink, engine:browser, compositor, protocol, config | landed | sway, scroll, macos | baseline | mostly-source | 0 |
| [input/touchpad-and-mouse-config](06-features/input/touchpad-and-mouse-config.md) | engine:input, config, session | landed | sway, scroll, hyprland, mutter, miracle-wm | baseline | mostly-source | 0 |
| [ipc-scripting/lua-and-shell-scriptability](06-features/ipc-scripting/lua-and-shell-scriptability.md) | any-shell, sdk, manganese | by-design-elsewhere | sway, scroll, miracle-wm, pinnacle | baseline | mostly-source | 0 |
| [ipc-scripting/read-model-and-events](06-features/ipc-scripting/read-model-and-events.md) | sdk, host, protocol, manganese | by-design-elsewhere | sway, scroll, niri, miracle-wm | baseline | mostly-source | 0 |
| [keyboard-and-text/keybinding-model](06-features/keyboard-and-text/keybinding-model.md) | engine:browser, compositor, sdk, manganese | landed | sway, scroll | baseline | mostly-source | 0 |
| [keyboard-and-text/shortcuts-inhibit](06-features/keyboard-and-text/shortcuts-inhibit.md) | engine:browser, compositor, protocol, sdk | landed-with-gap | sway, scroll, smithay | baseline | mostly-source | 0 |
| [keyboard-and-text/text-input-and-ime](06-features/keyboard-and-text/text-input-and-ime.md) | engine:browser, compositor, protocol, sdk, external | absent | sway, scroll, smithay, otto | baseline | mostly-source | 0 |
| [layout/floating-and-scratchpad](06-features/layout/floating-and-scratchpad.md) | manganese, sdk, compositor | landed | sway, scroll, cosmic, swayfx | baseline | mostly-source | 0 |
| [layout/layout-retention-across-reload](06-features/layout/layout-retention-across-reload.md) | any-shell, manganese, host | known-gap | sway | baseline | mostly-source | 1 |
| [layout/scrolling-layout](06-features/layout/scrolling-layout.md) | manganese, any-shell, engine:blink | absent | sway, scroll, niri, hyprland | baseline | mostly-source | 0 |
| [layout/tiling-splits-and-containers](06-features/layout/tiling-splits-and-containers.md) | manganese, sdk, engine:blink | landed | sway, scroll, niri, macos | baseline | mostly-source | 0 |
| [outputs/fractional-scale-and-viewporter](06-features/outputs/fractional-scale-and-viewporter.md) | engine:drm, engine:blink, compositor, protocol, config | landed | sway, scroll, smithay | baseline | mostly-source | 0 |
| [outputs/fullscreen-display-choice](06-features/outputs/fullscreen-display-choice.md) | compositor, protocol, sdk, manganese | landed | sway, scroll | baseline | mostly-source | 0 |
| [outputs/output-management-and-power](06-features/outputs/output-management-and-power.md) | engine:drm, compositor, config, session | landed-with-gap | sway, scroll, niri | baseline | mostly-source | 0 |
| [outputs/output-rendering-controls](06-features/outputs/output-rendering-controls.md) | engine:drm, compositor, protocol, config | unknown | sway, scroll | baseline | mostly-source | 0 |
| [outputs/per-output-color-and-gamma](06-features/outputs/per-output-color-and-gamma.md) | engine:drm, compositor, protocol, sdk | landed-with-gap | sway, scroll, kde | baseline | mostly-source | 0 |
| [pointer/cursor-shapes-and-constraints](06-features/pointer/cursor-shapes-and-constraints.md) | engine:aura, engine:drm, compositor, protocol, config | landed-with-gap | sway, scroll, smithay, hyprland | baseline | mostly-source | 0 |
| [pointer/hide-while-typing](06-features/pointer/hide-while-typing.md) | engine:aura, config | in-review | sway, scroll, ash, macos, hyprland, aura | baseline | mostly-source | 0 |
| [seats/independent-seat](06-features/seats/independent-seat.md) | engine:input, engine:browser, compositor, protocol, any-shell, external | absent | sway, scroll, niri, kitty | baseline | mostly-source | 0 |
| [seats/libseat-and-session](06-features/seats/libseat-and-session.md) | engine:input, engine:drm, session, external | by-design-elsewhere | sway, scroll, niri | baseline | mostly-source | 0 |
| [session/idle-and-lock](06-features/session/idle-and-lock.md) | engine:input, engine:drm, compositor, protocol, manganese, session | landed | sway, scroll, smithay | baseline | mostly-source | 0 |
| [session/portals-and-sandboxing](06-features/session/portals-and-sandboxing.md) | engine:browser, compositor, protocol, sdk, manganese, external | landed | sway, scroll, smithay | baseline | mostly-source | 0 |
| [session/shell-reload-and-recovery](06-features/session/shell-reload-and-recovery.md) | engine:browser, host, compositor, config, any-shell, session | landed | sway, scroll | baseline | mostly-source | 0 |
| [session/startup-commands](06-features/session/startup-commands.md) | config, session | landed | sway, scroll, niri | baseline | mostly-source | 0 |
| [session/xdg-activation-and-focus-requests](06-features/session/xdg-activation-and-focus-requests.md) | engine:browser, compositor, protocol, sdk, manganese | landed | sway, scroll | baseline | mostly-source | 0 |
| [shell-surfaces/layer-shell-equivalents](06-features/shell-surfaces/layer-shell-equivalents.md) | engine:blink, compositor, sdk, any-shell | by-design-elsewhere | views, scroll | n/a | mostly-source | 0 |
| [shell-surfaces/popup-and-popover-boundaries](06-features/shell-surfaces/popup-and-popover-boundaries.md) | engine:aura, engine:blink, sdk, manganese | unknown | aura, exo | n/a | mixed | 0 |
| [terminal/kitty-over-ssh-and-latency](06-features/terminal/kitty-over-ssh-and-latency.md) | engine:input, compositor, protocol, session, external | unknown | kitty | baseline | mixed | 0 |
| [terminal/kitty-wayland-protocols](06-features/terminal/kitty-wayland-protocols.md) | engine:input, compositor, protocol, manganese, external | unknown | kitty, sway, scroll | baseline | mostly-source | 0 |
| [windows/content-scaling](06-features/windows/content-scaling.md) | engine:browser, engine:blink, manganese, compositor | absent | scroll | baseline | mostly-source | 0 |
| [windows/criteria-and-window-rules](06-features/windows/criteria-and-window-rules.md) | manganese, any-shell, sdk | absent | sway, scroll, pinnacle, jay, hyprland, niri | baseline | mostly-source | 0 |
| [windows/focus-and-urgency](06-features/windows/focus-and-urgency.md) | manganese, sdk, compositor, engine:blink | landed | sway, scroll, mutter, hyprland | baseline | mostly-source | 0 |
| [windows/marks-and-advanced-marking](06-features/windows/marks-and-advanced-marking.md) | manganese, any-shell, sdk | absent | sway, scroll | baseline | mostly-source | 0 |
| [windows/native-state-requests](06-features/windows/native-state-requests.md) | compositor, protocol, sdk, manganese, engine:blink | landed | sway, scroll | baseline | mostly-source | 0 |
| [workspaces/overview](06-features/workspaces/overview.md) | manganese, engine:blink | landed | scroll, niri, ash | baseline | mostly-source | 0 |
| [workspaces/workspace-model-and-homes](06-features/workspaces/workspace-model-and-homes.md) | manganese, any-shell, compositor | landed | sway, scroll, niri, ash, pinnacle, jay | baseline | mostly-source | 0 |
| [workspaces/workspace-switching-and-slides](06-features/workspaces/workspace-switching-and-slides.md) | manganese, engine:blink | landed | sway, scroll | baseline | mostly-source | 0 |
