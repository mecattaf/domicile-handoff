# Index

One line per page, grouped by folder. Tables and row data live in `rows/` folders beside their pages.

## 00-overtures

- [Blur across compositor and page boundaries](00-overtures/blur.md): Blur depends on which scene the effect can sample.
- [The terminal as a Wayland client](00-overtures/terminal.md): Kitty is an ordinary native Wayland client.

## 01-chromium-lineage

- [CADE: Ash and Aura on generic Linux DRM](01-chromium-lineage/cade.md): CADE explored Chromium as the desktop compositor: Ash supplied window-management machinery, Ozone supplied DRM presentation and Exo hosted Wayland applications.
- [COWL revived and the patch campaign](01-chromium-lineage/cowl-revived.md): The 2026-06-16 revival restored the browser-and-shell client boundary under niri.
- [COWL: Chromium as a layer-shell client](01-chromium-lineage/cowl.md): COWL was a Chromium Content API embedder and later a browser-plus-shell fork.
- [CUBS: one browser process, a native Views shell](01-chromium-lineage/cubs.md): The June 29 CUBS design argument described a tier-2 Chromium fork whose browser process also hosted native Views shell surfaces under niri.
- [Domicile: the shipped form recognized in November](01-chromium-lineage/domicile-recognition.md): A study written on 2026-11-04 recognized Domicile and Manganese as a shipped implementation of the recurring Chromium-desktop idea.
- [Headwaters: a web shell with a typed namespace](01-chromium-lineage/headwaters.md): The December 2025 web-shell ground truth describes a transparent web interface in a native host, separated from system services by a typed namespace.
- [The July 2026 native-shell floor](01-chromium-lineage/july-native-shell-floor.md): The July continuation rebuilt a native Views shell on Chromium 150 under niri.
- [Chromium builds on Nix](01-chromium-lineage/mechanisms/chromium-on-nix.md): The build notes separate a source-build environment from distribution packaging.
- [Layer-shell client and compositor ownership](01-chromium-lineage/mechanisms/layer-shell-client-and-compositor.md): A layer-shell client submits its own pixels and requests anchoring, layer and exclusive space.
- [Ozone platform work](01-chromium-lineage/mechanisms/ozone-platform-work.md): The lineage used Ozone for two different jobs: creating Wayland client surfaces and presenting a Chromium-owned scene directly through DRM.
- [Tier-2 forks and rebase cost](01-chromium-lineage/mechanisms/tier-two-fork-rebase-cost.md): The CUBS design argument retained the browser and added shell roles.
- [The typed page/host seam](01-chromium-lineage/mechanisms/typed-page-host-seam.md): The historical seam exposed system state, asynchronous operations and subscriptions to a web shell while native code owned privileged resources.
- [The unified-binary insight](01-chromium-lineage/mechanisms/unified-binary.md): The earlier browser-and-shell designs aimed to share Chromium's browser services, GPU process and native toolkit.
- [Views on layer-shell](01-chromium-lineage/mechanisms/views-on-layer-shell.md): The 2026-06-30 result reports a native Views Widget attached to a layer-shell-backed Aura root.
- [The web-shell surface ceiling](01-chromium-lineage/mechanisms/web-shell-ceiling.md): The earlier web shell was clipped to each host surface.
- [A 2026-10-28 Chromium/Aura lineage report](01-chromium-lineage/october-aura-report.md): The October report reconstructed the compositor lineage while preserving the difference between original constraints, contemporary runtime reports and later interpretation.
- [quickshellX / webshell: web content under niri](01-chromium-lineage/quickshellx-webshell.md): quickshellX added QtWebEngine and QtWebChannel to a Quickshell host.
- [REVOLUTION: a purpose-built Chromium embedder](01-chromium-lineage/revolution.md): REVOLUTION reframed the shell host as a Chromium Content API embedder with direct Ozone work for Wayland surface roles.
- [views-shell and the cowl:// study](01-chromium-lineage/views-shell-and-cowl-scheme.md): The October study separated a browser-free Views shell client from a web system page.

## 02-aura-ash-exo

- [Ash desks](02-aura-ash-exo/ash-desks.md): Ash desks supply membership, lifecycle observers, activation and movement.
- [Ash overview](02-aura-ash-exo/ash-overview.md): Ash overview transforms live windows and transient descendants, retains restoration state and mirrors minimized windows.
- [Ash split view and window cycling](02-aura-ash-exo/ash-splitview-cycle.md): Split view constrains window bounds.
- [Ash system UI, shields and wallpaper](02-aura-ash-exo/ash-sysui-blur-wallpaper.md): Ash background shields and wallpaper controllers coordinate layer effects, lifecycle and ChromeOS policy.
- [Ash window states](02-aura-ash-exo/ash-wm-states.md): Ash records state transitions, restore geometry and client-controlled bounds changes; these are ChromeOS-gated controller mechanisms.
- [Aura window-management core](02-aura-ash-exo/aura-wm-core.md): Aura windows, transient parenting, modality, capture and focus are engine mechanisms; Ash policies that call them are separate donors.
- [Capture-mode coverage and current capture delivery](02-aura-ash-exo/capture-mode-gap.md): The July lift survey did not fully inventory the Ash capture-mode subtree.
- [Compositor blur and animation](02-aura-ash-exo/compositor-blur-anim.md): Aura layers supply filter properties and animation machinery.
- [Display management](02-aura-ash-exo/display-mgmt.md): Chromium separates display identity, stored topology and platform configuration.
- [Engine pin checks (table)](02-aura-ash-exo/engine-pin-checks-table.md): Generated from `rows/engine-pin-checks.tsv`; 20 rows.
- [Exo core](02-aura-ash-exo/exo-core.md): Exo models surfaces, buffers, subsurfaces, shell roles and security delegation inside Chromium.
- [Exo Wayland server and Ozone client](02-aura-ash-exo/exo-wayland.md): Exo registers server globals; Ozone Wayland binds client globals.
- [Input and gestures](02-aura-ash-exo/input-gestures.md): Ash recognizers and cursor rules combine reusable behavior with ChromeOS policy.
- [Views shell and strip behavior](02-aura-ash-exo/views-shell-vtabs.md): Views and Chrome strip controllers preserve interaction and lifetime rules.

## 03-wayland-protocols

- [core protocol collection](03-wayland-protocols/collections/core.md): Core objects and seat-created input resources.
- [ext protocol collection](03-wayland-protocols/collections/ext.md): Ext-prefixed desktop contracts and historical draft names, with lifecycle recorded per row.
- [kde protocol collection](03-wayland-protocols/collections/kde.md): KDE protocol compatibility surfaces.
- [stable protocol collection](03-wayland-protocols/collections/stable.md): Stable protocol specifications outside the core wire schema.
- [staging protocol collection](03-wayland-protocols/collections/staging.md): Staging, legacy unstable and custom schemas, with lifecycle recorded separately.
- [wlr protocol collection](03-wayland-protocols/collections/wlr.md): Protocols originating in the wlroots ecosystem.
- [xdg protocol collection](03-wayland-protocols/collections/xdg.md): Desktop-shell, activation and metadata protocols.
- [zcr protocol collection](03-wayland-protocols/collections/zcr.md): ChromeOS and Exo vendor protocols, including Aura wire surfaces.
- [Wayland protocol coverage at the pin](03-wayland-protocols/coverage-map.md): Each row records a protocol or a separately registered global identity.
- [Capture protocols and portal capture](03-wayland-protocols/topics/capture-and-screencopy.md): Portals mediate a user's source selection.
- [Per-output color and gamma](03-wayland-protocols/topics/color-and-gamma.md): A gamma ramp changes the output hardware transfer table.
- [DRM lease and display ownership](03-wayland-protocols/topics/drm-lease.md): A DRM lease gives a client direct use of selected display resources, commonly for a dedicated headset.
- [Text input, input methods and virtual keyboards](03-wayland-protocols/topics/input-method-and-virtual-keyboard.md): Text composition, an input-method service and a virtual keyboard are different protocol roles.
- [Layer-shell and exclusive zones](03-wayland-protocols/topics/layer-shell-and-exclusive-zones.md): Layer-shell lets an independent Wayland client request an anchored desktop surface, a stacking layer, keyboard-interactivity policy and reserved output space.
- [Seats and independent input participants](03-wayland-protocols/topics/seats-and-multiseat.md): The word seat names three separate mechanisms: permission to use devices, a group of logical input devices and focus, or a protocol that creates a temporary logical seat.
- [A compositor entry on Wayland Explorer](03-wayland-protocols/wayland-explorer-entry.md): Wayland Explorer publishes protocol documentation and a compositor support comparison.

## 04-sway-family

- [Any1 branch investigations](04-sway-family/any1-branch.md): The Any1 branch catalog mixes upstreamed device and seat work with experimental focus metadata and per-container state.
- [Scroll discussion findings](04-sway-family/dawsers-threads.md): The preserved discussion log records the boundaries between scroll's compositor mechanisms, Lua policy and external shell behavior.
- [Animations and feel](04-sway-family/scroll-parity/animations-and-feel.md): 179 source rows are indexed in the parity table.
- [Bar, status and tray](04-sway-family/scroll-parity/bar-and-status.md): 92 source rows are indexed in the parity table.
- [Binding modes and shortcuts](04-sway-family/scroll-parity/binding-modes.md): 107 source rows are indexed in the parity table.
- [Client and protocol contracts](04-sway-family/scroll-parity/client-and-protocol-contracts.md): 69 source rows are indexed in the parity table.
- [Command syntax and process configuration](04-sway-family/scroll-parity/command-and-process-config.md): 144 source rows are indexed in the parity table.
- [Content scaling](04-sway-family/scroll-parity/content-scaling.md): 13 source rows are indexed in the parity table.
- [Continuous gesture scrolling](04-sway-family/scroll-parity/continuous-gesture-scrolling.md): 14 source rows are indexed in the parity table.
- [Scope of the parity rows](04-sway-family/scroll-parity/coverage.md): The parity table retains 2,006 distinct SW/SC identifiers.
- [Criteria and window rules](04-sway-family/scroll-parity/criteria-and-window-rules.md): 50 source rows are indexed in the parity table.
- [Fit, align and center](04-sway-family/scroll-parity/fit-align-and-center.md): 48 source rows are indexed in the parity table.
- [Focus and urgency](04-sway-family/scroll-parity/focus-and-urgency.md): 79 source rows are indexed in the parity table.
- [Idle and lock](04-sway-family/scroll-parity/idle-and-lock.md): 11 source rows are indexed in the parity table.
- [IPC observations and events](04-sway-family/scroll-parity/ipc.md): 372 source rows are indexed in the parity table.
- [Jump labels](04-sway-family/scroll-parity/jump.md): 26 source rows are indexed in the parity table.
- [Lua scripting](04-sway-family/scroll-parity/lua-scripting.md): 127 source rows are indexed in the parity table.
- [Marks](04-sway-family/scroll-parity/marks.md): 19 source rows are indexed in the parity table.
- [Notification helper contracts](04-sway-family/scroll-parity/notification-helper.md): 107 source rows are indexed in the parity table.
- [Overview](04-sway-family/scroll-parity/overview.md): 19 source rows are indexed in the parity table.
- [Scratchpad and floating windows](04-sway-family/scroll-parity/scratchpad-and-floating.md): 20 source rows are indexed in the parity table.
- [Scrolling rows and columns](04-sway-family/scroll-parity/scrolling-rows-and-columns.md): 114 source rows are indexed in the parity table.
- [Seat and input configuration](04-sway-family/scroll-parity/seat-and-input-config.md): 137 source rows are indexed in the parity table.
- [Tiling splits and containers](04-sway-family/scroll-parity/tiling-splits-and-containers.md): 31 source rows are indexed in the parity table.
- [Trails, selections and saved spaces](04-sway-family/scroll-parity/trails-and-advanced-marking.md): 50 source rows are indexed in the parity table.
- [Virtual outputs](04-sway-family/scroll-parity/virtual-outputs.md): 12 source rows are indexed in the parity table.
- [Window state and fullscreen](04-sway-family/scroll-parity/window-state-and-fullscreen.md): 32 source rows are indexed in the parity table.
- [Workspaces and output rules](04-sway-family/scroll-parity/workspaces-and-outputs.md): 134 source rows are indexed in the parity table.
- [Sway-next: the recorded compositor fork](04-sway-family/sway-next.md): Sway-next combined scroll's layout with sway-compatible names and commands, swayfx-style effects, and additional window and seat state.
- [Sway contracts beside the scroll layout](04-sway-family/sway-semantics.md): Sway's command set covers window identity, matching, modes, workspace placement and external clients as well as layout.
- [Swayfx effects and compositor boundaries](04-sway-family/swayfx.md): Swayfx adds effect controls and lifecycle changes to sway.

## 05-graveyard

- [The July Aura/Ash/Exo lift map](05-graveyard/aura-lift-july.md): A component survey classified reusable Chromium machinery, ChromeOS-coupled implementation and floor behavior requiring fresh expression.
- [CADE](05-graveyard/cade.md): CADE investigated Ash/Aura as the compositor, with DRM presentation and Exo application hosting on generic Linux.
- [COWL, its revival and patch campaign](05-graveyard/cowl.md): COWL was a native Chromium embedder and browser-plus-shell client under niri.
- [CUBS and the July native-shell floor](05-graveyard/cubs.md): The CUBS design argument combined browser services and native Views shell surfaces in a tier-2 Chromium fork under niri.
- [Electron and CEF as embedding donors](05-graveyard/electron-cef.md): The March study compared Electron, CEF offscreen rendering and a direct Chromium embedder for desktop shell surfaces.
- [GPUiX and the return to sway](05-graveyard/gpuix-aura-to-hyprland-and-return.md): The 2026-10-13..22 investigation explored a native React/Solid shell over a wlroots compositor.
- [Headwaters](05-graveyard/headwaters.md): A web shell in a native host exposed system data and operations through a typed namespace.
- [A multiplayer desktop on sway and VNC](05-graveyard/multiplayer-desktop-2022.md): A 2022-23 desktop combined sway with VNC access.
- [The October Otto native-shell investigation](05-graveyard/otto-surge.md): A 2026-10-23..28 investigation examined Otto as a patched compositor base and as the host for a native shell.
- [Otto on a virtual terminal](05-graveyard/otto-trial.md): A 2026-10-06 study explored Otto as a Skia-based desktop on a virtual terminal, alongside a retained scroll session.
- [quickshellX / webshell](05-graveyard/quickshellx-webshell.md): A QtWebEngine host rendered a web shell under niri.
- [The sway-next compositor study](05-graveyard/sway-next.md): The 2026-10-22..23 sway-next study explored a compositor fork that kept sway-compatible commands while retaining scroll layout, overview, jump and Lua semantics.
- [views-shell and the cowl:// study](05-graveyard/views-shell-and-cowl-scheme.md): The October study separated a browser-free Views client from a web system page and its privileged browser seam.
- [Window-manager scoping in October 2026](05-graveyard/wm-scoping-2026-10.md): The 2026-10-06 scoping compared scroll, niri and a standalone Chromium Views shell.
- [A workspace sidebar beside sway](05-graveyard/workspace-sidebar-spike.md): A 2026-10-26..11-03 spike kept applications in Chrome while a separate native panel sat beside sway.

## 06-features

- [Desks and virtual desktops](06-features/aura-wm/desks-and-virtual-desktops.md): A workspace groups windows, remembers activation state and defines how windows move between groups.
- [Display topology and configuration](06-features/aura-wm/display-management.md): Display configuration joins connector identity, modes, scale, rotation and relative placement.
- [Overview over live windows](06-features/aura-wm/overview-mode.md): Overview changes the presentation of existing windows while preserving the state required to leave overview.
- [Split view and window cycling](06-features/aura-wm/split-view-and-cycle.md): Cycling previews a candidate window and either commits that selection or restores the original activation.
- [Window state transitions](06-features/aura-wm/window-states.md): Restoring a window can require both earlier geometry and a compatible prior state.
- [Notifications, history and actions](06-features/bar-and-notify/notifications.md): Notifications remain available in history after their toast expires, and actions return to the application that sent them.
- [Quick settings and clipboard panels](06-features/bar-and-notify/quick-settings-and-clipboard-panels.md): Quick settings exposes system controls through reusable shell packages; the clipboard panel displays compositor-held history.
- [Status bar modules and application tray](06-features/bar-and-notify/status-bar-and-tray.md): The status bar combines independent desktop modules, while application tray items carry their own actions and menus.
- [Keybinding modes and chord ownership](06-features/bindings-and-modes/binding-modes.md): A binding mode changes which action a grabbed chord or gesture runs, while the engine retains ownership of matching the physical input.
- [Browser prompts, pickers and error pages in the shell](06-features/browser-windows/chrome-dialogs-in-shell.md): A browser guest reports requests to its owning shell element.
- [Window groups and strip behavior](06-features/browser-windows/tab-groups-and-strips.md): A group preserves window membership while its visible strip changes state.
- [Web apps and engine-owned app windows](06-features/browser-windows/web-apps-and-app-windows.md): A web app is a URL with a desktop entry.
- [Nix engine artifacts and build identity](06-features/build-and-packaging/nix-and-engine-builds.md): A fixed desktop revision pins an exact engine artifact.
- [Foreign toplevel lists and window management](06-features/capture/foreign-toplevel-and-taskbar.md): Foreign-toplevel protocols let a client enumerate native Wayland windows and, where a management protocol permits, request window actions.
- [Screen and window capture](06-features/capture/screencopy-and-image-capture.md): Screen and window capture share the compositor casting machinery through two interfaces: permission-mediated portals and direct Wayland capture protocols. upstream documentation [docs]
- [Clipboard, primary selection and drag-and-drop](06-features/clipboard-dnd/data-control-and-primary-selection.md): The compositor owns clipboard selections and a memory-only history.
- [Background blur behind windows and shell surfaces](06-features/effects/backdrop-blur.md): A translucent shell element can filter the scene behind it.
- [Window clipping, shadows and dimming](06-features/effects/corner-radius-shadows.md): Clipping changes the visible window content.
- [Window animation lifetime and scene transitions](06-features/effects/window-animations.md): Windows can move, fade, change workspace and leave the scene while their content remains available for the transition.
- [Chromium fork and patch discipline](06-features/engine-mechanisms/engine-fork-and-patch-discipline.md): A fork has an upstream code revision, an overlay and patch set, and a built artifact identity.
- [GPU and software rendering backends](06-features/engine-mechanisms/gpu-and-render-backends.md): Buffer import, scene drawing and presentation are separate parts of the rendering path.
- [Typed page and host calls](06-features/engine-mechanisms/page-host-seam.md): A page makes a typed host call while native presentation and window policy retain their own contracts.
- [Scroll feel and animation continuity](06-features/feel-and-animation/scroll-feel-parity.md): Scroll feel depends on event phases, target selection, direct-manipulation behavior and animation interruption, as well as the visible easing curve.
- [Workspace slide and bounce continuity](06-features/feel-and-animation/workspace-slide-bounce.md): Workspace transitions preserve a continuous slide across repeated switches and keep the switcher’s feedback aligned with the selected workspace.
- [Gesture phases and ownership](06-features/gestures/gesture-claim-api.md): A shell claims a gesture by kind and count, then receives begin, updates and end.
- [Multi-finger swipes and holds](06-features/gestures/multi-finger-swipe-and-hold.md): The shell can follow a multi-finger gesture continuously and run a binding when its final direction is known.
- [Touchpad pinch to native windows and browser pages](06-features/gestures/touchpad-pinch.md): A pinch retains its native phases until a shell claims it, a Wayland client receives it, or a browser page uses it for page zoom. upstream documentation [docs]
- [Touchscreen contacts and display-edge swipes](06-features/gestures/touchscreen-and-edge.md): Direct contacts reach the touched Wayland window.
- [Disable touchpads while typing](06-features/input/disable-while-typing.md): Typing suppression has two distinct policies at the pin: the engine can pause touchpad events with its own guard, or delegate suppression to libinput.
- [Finger scrolling and post-lift momentum](06-features/input/scroll-feel-and-momentum.md): A finger scroll retains its source and independent axis stops.
- [Configure touchpads and mice](06-features/input/touchpad-and-mouse-config.md): Touchpad and mouse settings reach the tty engine through a validated configuration snapshot.
- [Lua semantics and reusable shell modules](06-features/ipc-scripting/lua-and-shell-scriptability.md): Scroll embeds Lua for layout operations and persistent script state; Domicile exposes a JavaScript shell contract and independently reusable Manganese packages.
- [Desktop state and change events](06-features/ipc-scripting/read-model-and-events.md): The public shell API exposes typed desktop state and change events; terminal commands enter the shell’s own command handler.
- [Native chords and the typed key table](06-features/keyboard-and-text/keybinding-model.md): The shell claims native key chords and receives typed shortcut events even when a browser guest has keyboard focus.
- [Shortcut inhibition for remote desktops and games](06-features/keyboard-and-text/shortcuts-inhibit.md): A focused client can inhibit compositor shortcuts so its remote desktop or game receives the key combinations.
- [Text input, input methods and virtual keyboards](06-features/keyboard-and-text/text-input-and-ime.md): Text-input, input-method and virtual-keyboard protocols cover different parts of text entry: application editing state, an input-method service and injected key events.
- [Floating groups and the scratchpad](06-features/layout/floating-and-scratchpad.md): A floating group moves independently of the tiled tree; the scratchpad hides a window and recalls it without consuming a workspace slot.
- [Window placement across shell reloads](06-features/layout/layout-retention-across-reload.md): A shell reload preserves client windows while replacing the page that holds the shell’s layout state.
- [Scrolling rows and columns](06-features/layout/scrolling-layout.md): A scrolling layout keeps useful window sizes while a viewport moves through columns and the windows within them.
- [Nested splits and container layouts](06-features/layout/tiling-splits-and-containers.md): A layout tree groups windows into splits, tabs and stacks, with focus and movement defined at each level.
- [Fractional scaling and client viewports](06-features/outputs/fractional-scale-and-viewporter.md): Fractional output scale separates physical buffer pixels from desktop logical coordinates.
- [Honor a native fullscreen display request](06-features/outputs/fullscreen-display-choice.md): A native client that names an output in its fullscreen request is placed on that screen's current workspace.
- [Output profiles and display power](06-features/outputs/output-management-and-power.md): Display profiles determine which outputs participate, where they appear, their scale and rotation.
- [Output rendering controls](06-features/outputs/output-rendering-controls.md): Output filtering, subpixel order, adaptive sync, render depth, HDR, tearing and render budgets are separate choices in the pinned Sway and Scroll implementations.
- [Per-output night light and gamma ramps](06-features/outputs/per-output-color-and-gamma.md): Each monitor has its own gamma ramp.
- [Cursor shape, motion and constraints](06-features/pointer/cursor-shapes-and-constraints.md): Named cursor shapes are served through the cursor-shape protocol; the tty engine selects and draws the image at the active display scale and rotation.
- [Hide the pointer while typing and after inactivity](06-features/pointer/hide-while-typing.md): The reviewed change hides the pointer image during typing or after an inactivity timeout, while keeping pointer focus and hover intact. #1945 [docs]
- [Wayland protocol coverage map](06-features/protocols/coverage-map.md): Rows and findings only.
- [Independent logical seats](06-features/seats/independent-seat.md): A logical seat groups input devices, a pointer and keyboard focus for an independent participant.
- [Device access and the tty session](06-features/seats/libseat-and-session.md): Device access is a session service concern.
- [Idle blanking and the screen lock](06-features/session/idle-and-lock.md): The compositor counts real input, enforces the desktop lock and turns connectors off for idle blanking.
- [Portals and sandboxed clients](06-features/session/portals-and-sandboxing.md): Portals connect application requests to desktop permission and selection flows.
- [Shell reload and component recovery](06-features/session/shell-reload-and-recovery.md): The launcher watches shell configuration and supervises the engine and compositor separately.
- [Startup commands and shell reload](06-features/session/startup-commands.md): Each compositor start runs its configured command vectors after publishing the session.
- [Activation tokens and focus requests](06-features/session/xdg-activation-and-focus-requests.md): Activation tokens distinguish a timely user-driven focus handover from a background request.
- [Panels and overlays in one desktop page](06-features/shell-surfaces/layer-shell-equivalents.md): A shell places panels and overlays within the desktop page.
- [Popup membership and dismissal](06-features/shell-surfaces/popup-and-popover-boundaries.md): A popup belongs to a parent interaction even when it occupies a separate surface.
- [Local terminal rendering and remote SSH transport](06-features/terminal/kitty-over-ssh-and-latency.md): A local Kitty window renders the terminal while SSH carries bytes to and from a remote process.
- [Kitty as an ordinary Wayland client](06-features/terminal/kitty-wayland-protocols.md): Kitty owns its terminal state, renderer, selection and child process.
- [Per-window content scale](06-features/windows/content-scaling.md): Content scaling changes a window’s content density independently of monitor scale and overview transforms.
- [Window criteria and opening rules](06-features/windows/criteria-and-window-rules.md): Criteria select windows by attributes, and rules apply actions when matching windows appear or change relevant properties.
- [Pointer focus and activation policy](06-features/windows/focus-and-urgency.md): Focus policy decides which window receives keys after pointer movement, directional navigation, closing and activation requests.
- [Window marks, selections and trails](06-features/windows/marks-and-advanced-marking.md): Marks name containers; selections and trails add multi-window operations and ordered return paths.
- [Native window state requests](06-features/windows/native-state-requests.md): A native client requests maximize or fullscreen state; the shell decides placement and reports the states represented by that placement.
- [Workspace and window overview](06-features/workspaces/overview.md): Overview shows live windows in workspace tiles and supports selecting, moving and steering among those tiles.
- [Workspaces and screen homes](06-features/workspaces/workspace-model-and-homes.md): Workspaces group windows, while a screen home records where a workspace is shown or returns when selected.
- [Workspace switching and slides](06-features/workspaces/workspace-switching-and-slides.md): Workspace switching changes the shown workspace while a screen-local slide keeps the transition continuous.

## 07-living-on-domicile

- [Living symptoms, second table](07-living-on-domicile/living-reports-table.md): Generated from `rows/living-reports.tsv`; 114 rows.
- [Living (table)](07-living-on-domicile/living-table.md): Generated from `rows/living.tsv`; 39 rows.

## Top level

- [Isolated findings from material otherwise outside the map](95-gems-index.md): Single facts from studies that are otherwise outside the map, grouped by the feature page they bear on.
