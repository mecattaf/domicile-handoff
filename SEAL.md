# Seal

Sealed 2026-11-09.

## Pins

| What | Pin |
|---|---|
| Domicile `main` | `996d8b628439a5d9b12204b49c9d472f249666bc` (2026-11-08) |
| Domicile engine (Chromium fork base) | `2493812cc0621330895498ea437bde415944c203` (Chromium 159.0.8137.0) |
| Chromium the earlier studies read | 150.0.7871.124 (150.0.7871.96 for one component check) |
| Smithay (Domicile's fork, from `Cargo.lock` at the pin) | `fb981077def1405300e69a5993e9789768e772aa` |
| sway | `1652c54b73f67df17b7b4ab0b0f7048204aa8104` |
| scroll | `b6586d273fed7d187744b7eaec2c234386a029fa` |
| Earlier Domicile snapshots named by date | `965dea4077415691f7ad409d5d5b5df82115575c` (2026-11-06), `75708d7638002e1bba421cf83f6927298dbc1c3a` (2026-11-07); historical only |
| Wayland core schema | wayland 1.26.0 |

## Integrity

- `MANIFEST.sha256` lists every file in this tree except itself and this page, with its SHA-256. From the repository root, `sha256sum -c MANIFEST.sha256` verifies it.
- SHA-256 of the manifest: `dde177aa18ebd8e4169d9521c95bb8a25c39853f311997a2af5db3e2175ac8ce`
- Provenance commitment: `818e45c7ebc446aa66e1345c79aab8de777175a66f1ce5c5d73cae8dee6cbb86` (SHA-256 of a salt and a deterministic archive of the private source index; sources are cited by opaque id).
- `MANIFEST.sha256.sig` is an OpenSSH signature over the manifest in the `domicile-handoff` namespace; `allowed_signers` holds the public key. Verify with:
  `ssh-keygen -Y verify -f allowed_signers -I domicile-handoff -n domicile-handoff -s MANIFEST.sha256.sig < MANIFEST.sha256`
