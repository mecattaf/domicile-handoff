# Labels and vocabulary

## Evidence labels (inline, after a claim)
- `[measured]`: ran, booted, timed or observed on Domicile at the stated pin, on a basis the maintainer can check.
- `[source]`: read in pinned public source; a permalink accompanies it.
- `[docs]`: upstream documentation or the notes say so; not tested.
- `[inferred]`: reasoned, not checked.

## `domicile_status` (at the pin)
- `landed`: merged PR and a doc permalink.
- `landed-with-gap`: landed, with a KNOWN-GAPS line or the PR's own known gaps.
- `in-review`: open PR.
- `known-gap`: KNOWN-GAPS line, no PR.
- `by-design-elsewhere`: a Domicile doc assigns the need to a different mechanism.
- `stated-out-of-scope`: a doc says no.
- `absent`: no trace at the pin; absence is a disposition, never a defect.
- `unknown`: not checked.
- `n/a`: the page is not about a single Domicile-facing feature.

## `layers` (where the mechanism lives)
`engine:input` (ozone evdev, libinput) · `engine:aura` (UI thread, `aura::Env`, ui/wm) · `engine:blink` (`<app>`, `<webview>`, html.css) · `engine:viz` · `engine:drm` (modeset, gamma, cursor planes) · `engine:browser` (WebViewGuest, Chrome UI) · `host` (routing, ownership) · `compositor` (Smithay delivery, globals, portals, EIS) · `protocol` (wire) · `config` · `sdk` · `manganese` · `any-shell` · `system-apps` · `session` (host process, launch, tty session, Nix) · `external` (libinput, Smithay upstream, Chromium upstream, wayland-protocols, clients).

## `scroll_parity`
`baseline` (any sway or scroll semantic) · `beyond` (swayfx, sway-next, any1 additions) · `n/a`.

## `donor_availability` (Chromium-side donors)
`in-engine-build` (compiled into the Linux chrome build: ui/aura, ui/wm/core, ui/compositor, views) · `rule-portable` (ash or exo logic re-expressible in an overlay) · `chromeos-gated` (`ash/`, or `components/exo` behind `assert(is_chromeos)`) · `semantics-only` (Exo's server side informs; Smithay serves Wayland).

## `evidence_mix`
`mostly-measured` · `mostly-source` · `mixed` · `mostly-inferred`.

## `kind`
`feature` · `era` · `mechanism` · `protocol` · `collection` · `topic` · `family` · `graveyard` · `overture` · `living` · `shell-note` · `design-note`.

## Tags (at most 8 per page)
Prefixes: `layer:` · `status:` · `family:` · `donor:` · `era:` · `evidence:` · `scroll-parity:` · `protocol:` · `domicile:#NNNN` · `bucket:` (domicile).

## Lineage values
`measured` · `booted` · `prototyped` · `designed` · `surveyed` · `named` · `stopped(<reason>)` · `carried-to:<id>` · `sent-upstream:#N`.
