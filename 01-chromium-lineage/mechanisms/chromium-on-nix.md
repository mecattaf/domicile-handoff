---
id: 01-chromium-lineage/mechanisms/chromium-on-nix
title: "Chromium builds on Nix"
kind: mechanism
domicile_status: n/a
checked_at: 2026-11-09 https://github.com/cprussin/domicile/commit/996d8b628439a5d9b12204b49c9d472f249666bc
evidence_mix: mixed
bucket: domicile
tags: [bucket:domicile, evidence:docs]
sources: [S-c8fb36d42452, S-8b956f5955ba, S-22a1a515244f, S-d33daba0ca9c, S-1c58e1373c0b, S-514bff6c69a0, S-6ee2a1600602, S-7b49aa4bb943]
---

# Chromium builds on Nix

The build notes separate a source-build environment from distribution packaging. A package wrapping an existing engine artifact is not evidence of a reproducible source build. [docs]

## Eras and findings

- The July pipeline design staged one installation artifact and repackaged it for distribution channels. The archive records no completed nightly release. [docs]
- Its Nix build environment was exercised through generation and wiring at the old Chromium pin; the archive does not establish an end-to-end compile. [docs]
- The July native-shell report separately records a completed Chromium 150 build and live runtime limits. These are different evidence sets. [docs]
- The June runtime report records component-build changes relinking shared libraries while the executable could remain unchanged. Deployment identity therefore concerns the artifact closure. [docs]
- A June build result records a global fork flag reaching generated headers, conditional source and linking. Its initial ChromeOS assertion also fired in a Linux host-tool build: host and target conditions needed separate treatment. The linked artifact still left display qualification open. [docs]
- Generic findings include a derived runtime-dependency closure, explicit compiler-cache configuration and tool bootstrap as independent build prerequisites. [docs]

> Era and its assumptions: the June patch campaign and July 2026 native-shell floor, which assumed the project maintained a separate engine builder and distribution pipeline. For Domicile this probably adds little: Domicile already supplies its own engine releases and packaging. Recorded because source build, artifact closure and channel metadata are different contracts.


## Where it touches Domicile

[Engine-fork architecture](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/architecture/ENGINE-FORK.md), [the installation guide](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/INSTALL.md) and [shell packaging](https://github.com/cprussin/domicile/blob/996d8b628439a5d9b12204b49c9d472f249666bc/docs/SHELL-PACKAGING.md) describe Domicile's supported path. [docs]


## Sources

- S-22a1a515244f · 2026-11-08 · Archived Chromium builder and distribution pipeline
- S-d33daba0ca9c · 2026-11-08 · Build-environment and packaging comparison
- S-1c58e1373c0b · 2026-07-16 · Native shell build and runtime evidence
- S-8b956f5955ba · 2026-06-27 · Component-build deployment closure and source/runtime drift
- S-c8fb36d42452 · 2026-06-29 · Fork build-flag pipeline and host-versus-target toolchain correction
- S-514bff6c69a0 · 2026-11-09 pin check · Domicile engine integration and patch discipline
- S-6ee2a1600602 · 2026-11-09 pin check · Pinned shell packaging contract
- S-7b49aa4bb943 · 2026-11-09 pin check · Pinned installation and Nix module contract
