---
id: graveyard/otto-trial
title: Otto on a virtual terminal
kind: graveyard
layers: ["session", "compositor", "external"]
domicile_status: n/a
checked_at: "996d8b628439a5d9b12204b49c9d472f249666bc 2026-11-09"
bucket: domicile
evidence_mix: mostly-source
sources: ["S-0b1c790ab7ac", "S-473ce8c1815e", "S-dfefabad181b", "S-ef23fc7381cf", "S-e10e394fdecf", "S-74bd8f626d89", "S-27f028d58ee2"]
tags: ["bucket:domicile", "evidence:docs", "donor:otto"]
---

# Otto on a virtual terminal

A 2026-10-06 study explored Otto as a Skia-based desktop on a virtual terminal, alongside a retained scroll session. The preserved packaging study described a separate session target, explicit display-environment ownership and a reversible session choice. [docs]

## What it established

- The integrated session package and system image were built with a notification-based startup boundary. The record explicitly says the image was neither activated nor booted. [docs]
- The shell study distinguished compositor-resident desktop machinery from separate Otto-kit client processes. The compositor and the clients used distinct rendering contexts. [docs]
- A source-level configuration comparison found no scrolling strip or corresponding fit and centering modes in the tiling model. It did not establish a running configuration port. [docs]
- The split-only tree and parser did not implement the broader tabbed and stacked layout specification; the tab-group change set remained a design. [docs]
- Headless shell observations did not establish hardware frame pacing or physical display recovery. [docs]

## Learnings

- A successful system-image build did not establish a working physical desktop. [docs]
- The active virtual-terminal session and display environment were part of compositor startup. [docs]
- Compositor-owned shell machinery and separately launched panels had different replacement boundaries. [docs]
- A Skia-based compositor did not make other Skia toolkits interchangeable with its shell. [docs]

## Ending and carry-forward

Work stopped for reasons outside this library's scope; the technical state supported by these sources was a built but unbooted session image and a source-level shell comparison. The sources retained here do not establish the early trial's physical outcome or its stopping cause. [docs]

The later [Otto investigation](otto-surge.md) records separate native-host, blur and physical-trial evidence. Its outcomes are not projected back onto the early trial.

## Sources

- S-0b1c790ab7ac · 2026-10-06 · Separate compositor-session packaging evaluation.
- S-473ce8c1815e · 2026-10-06 · Native compositor and client-shell boundary study.
- S-dfefabad181b · 2026-10-06 · Native-shell session scope and recorded limitations.
- S-ef23fc7381cf · 2026-10-06 · Configuration-schema and layout comparison without a compositor launch.
- S-e10e394fdecf · 2026-10-06 · Split-tree implementation and tabbed-layout design boundary.
- S-74bd8f626d89 · 2026-10-06 · Integrated session-image build without activation or boot.
- S-27f028d58ee2 · 2026-10-06 · Read-only session safety study without switching or compositor launch.
