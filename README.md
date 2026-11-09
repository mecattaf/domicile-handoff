---
id: README
title: Domicile handoff library
kind: collection
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
bucket: domicile
evidence_mix: mixed
tags: [bucket:domicile]
sources: []
---

# Domicile handoff library

A year of compositor and desktop-shell investigation, organized as reference material for Domicile and Manganese. It is a closed chapter shared in full: dated, labeled, and meant to be picked from, not followed. It ranks nothing and asks for nothing.

The pages were compiled from the contributor's notes with AI assistance. Every claim carries an evidence label, and every `[source]` claim a pinned link.

Every status is checked against one Domicile revision, `996d8b62` (2026-11-08). `SEAL.md` lists the pins and the manifest.

## How to read it

- `READING-GUIDE.md` splits the library into seven shards small enough to read in one sitting.
- `COVERAGE.md` has one row per feature page: layers, status at the pin, donors, scroll parity, evidence mix.
- `INDEX.md` lists every page; `INDEX-BY-LAYER.md`, `INDEX-BY-STATUS.md`, `INDEX-BY-DONOR.md` and `INDEX-BY-DOMICILE-DOC.md` regroup them.
- `LABELS.md` defines the evidence labels (`[measured]`, `[source]`, `[docs]`, `[inferred]`), the status values and the layer names used on every page. `GLOSSARY.md` names the eras and donors.
- `catalog.jsonl` is the machine-readable form of every page's front matter.
- Data lives in `rows/*.tsv` folders beside the pages that explain them; the TSV is the record, the page is the reading.

## Parts

- [Overtures](00-overtures/README.md): the terminal as a Wayland client, and blur across compositor and page boundaries.
- [Chromium lineage](01-chromium-lineage/README.md): era evidence and mechanism chapters.
- [Aura, Ash and Exo](02-aura-ash-exo/README.md): source availability and equivalence rows.
- [Protocol map](03-wayland-protocols/README.md): globals, versions, members and recorded limits.
- [sway and scroll](04-sway-family/README.md): semantic families, source rows and Domicile coverage at the pin.
- [Graveyard](05-graveyard/README.md): dated results, limits and what carried forward.
- [Feature families](06-features/README.md): one page per feature.
- [Living on Domicile](07-living-on-domicile/README.md): neutral symptoms and their upstream state.
- [Isolated findings](95-gems-index.md): single facts from studies otherwise outside the map.

## What the folders are

| Folder | Holds |
|---|---|
| `00-overtures` | Two longer pieces: the terminal as a Wayland client and remote transport, and background blur through sampling, filtering and presentation. |
| `01-chromium-lineage` | The eras of web-shell and Chromium-side compositor work, as dated pages, and the mechanism chapters they produced. |
| `02-aura-ash-exo` | What Chromium's Aura, ChromeOS's Ash and Exo offer, row by row, with build-gating and portability recorded per component. |
| `03-wayland-protocols` | The protocol coverage map: globals, versions and members across sway, scroll, Exo and Domicile at the pin, with topic pages (seats, color and gamma, capture, input methods, layer-shell equivalents, DRM lease). |
| `04-sway-family` | sway, scroll, swayfx, sway-next and any1 semantics, family by family, with Domicile coverage at the pin; scroll parity is the stated ideal. |
| `05-graveyard` | Each closed investigation: what it set out to do, what it proved, what it taught, and its ending. |
| `06-features` | One page per feature in the donor pattern: behavior, how donors do it, what the notes found, where it touches Domicile, lineage, sources. |
| `07-living-on-domicile` | Reproducible symptoms observed on Domicile, each tied to a feature page and to its upstream state at the pin. |

## Conventions

- Third-person, present tense; history only in `## Lineage` sections, in `01` and in `05`.
- Absence is a disposition ("not addressed as of the pin"), never a defect.
- Sources are cited by opaque id (`S-` plus twelve hex characters) and date; they are resolvable on request.
- Pages that touch programmatic desktop control carry one sentence marking that boundary as outside this library.

## License

MIT; see `LICENSE`. Linked and quoted third-party material keeps its own license; see `THIRD-PARTY-NOTICES.md`.
