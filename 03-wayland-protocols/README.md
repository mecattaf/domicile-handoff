---
id: protocols
title: Wayland protocol map
kind: family
family: protocols
layers: [compositor, protocol, external]
domicile_status: n/a
domicile_refs: []
checked_at: "2026-11-09"
scroll_parity: baseline
evidence_mix: mostly-source
boundary_note: false
bucket: domicile
tags: [layer:protocol, evidence:source, scroll-parity:baseline]
sources: [S-000764dce113, S-00ffae850184, S-014c456bb822, S-016ff15271ae, S-04faf44f2e81, S-06ca08859f6e, S-083adea7e7ad, S-0aa2cc30bb07, S-0b6c31dfe6bc, S-0d6c45f5a62b, S-1346c57ad82c, S-135ecf2b8dea, S-1389e89c68c6, S-1497ad1e40a4, S-14b8d68070ec, S-1570b41820c4, S-18a2678e3352, S-18ec6dfe35f6, S-1b81a86927e0, S-1c210691c0b8, S-1d7075319a3c, S-1edc2e41b6da, S-1f5025ab76ad, S-229f7c4d9b14, S-2822956bdf13, S-291dce183eb2, S-2b83da449b94, S-2ea0540e0f1d, S-2f9c9a88b407, S-3138604e8df0, S-32784ffd9a8d, S-33085e8f5a27, S-332161a6dfa1, S-3674591ca8e6, S-37cd947718b7, S-385433e1522c, S-3acbfb4907e7, S-3c995b7a34f5, S-3d63fd27af2d, S-3e03b7086707, S-3e37be57c91a, S-3f4edde82525, S-3f524db52413, S-3f5dbd0c9580, S-4378aebc583f, S-43af2b3af183, S-44598db24690, S-44be82b2b78e, S-46587779628d, S-46f8f6ded930, S-472d3a68890f, S-499a1aa22127, S-4aa1e405fd79, S-4cd685c56e61, S-4d83eed81e29, S-4fee70ea6452, S-507aa315e372, S-51292cdb8ce9, S-51497eee422b, S-534dcadd1369, S-548e570973fa, S-5532dadfd152, S-594f7c37cd88, S-5bb7550e6b56, S-5e4ce5d63275, S-5e57a72fa9cc, S-5eb8003b42fd, S-5f315b39ac7c, S-5fb9706b471e, S-5fcf9ba138c3, S-610c446f4416, S-654a5c549cb3, S-66930fd17085, S-66b2ff508daa, S-67ccf82fd285, S-687d7a1db0ce, S-6ba440ed6420, S-6fceeb23f0e9, S-703c0d408b29, S-70da4c3a822b, S-7170fa893ec4, S-71c6b5b6fa74, S-728ccd41be9e, S-74f557738f56, S-77d28cc4fbfe, S-782b0928e349, S-7a89a244a0f8, S-7b295c5a7393, S-7b31c5bb3be0, S-7cd85cb7ef61, S-7f8302736ae8, S-7fff5f5a9d7c, S-82b445bd2404, S-832c3939e13e, S-85c7cd8374aa, S-863c5efcb1a1, S-866b9f3f829d, S-88f5036dac2f, S-8a46216d2a3e, S-8bf472405b20, S-8c6da920d0f9, S-8e9e08836f9d, S-9081893da4f7, S-914380369a95, S-9175c1754b40, S-9297edaee809, S-92d9911f0704, S-9586d9691bef, S-961f39214831, S-98268a478f8c, S-9a9f91803d7e, S-9e7d963ab743, S-9ecc381476d7, S-a04ed2a57dc2, S-a103e36fe4b1, S-a19d3732cf8f, S-a1fd4d71b416, S-a3671b8e50eb, S-a542cf9c1edb, S-a6589c3205d1, S-a89a584c0175, S-a90d2490bf23, S-ae7fe663e21e, S-af6e4774be6e, S-b2415d10a041, S-b32e8bae0905, S-b4058c3be3bd, S-b57d7ad8f1ae, S-b61833f7b205, S-b824688e0e5c, S-b9df7238ba4e, S-bad77f83f7f6, S-bc6dca48e478, S-bd39d7cfd9cb, S-c2596d00b100, S-c34c6ce697a3, S-c4bef886de89, S-c4fb695b7c41, S-c5d3db1884f1, S-c5dec4346034, S-c977034319b9, S-ca1441e8ae06, S-ca6aadf58810, S-cc860987e54f, S-ccf976691aa5, S-ce8dab4e8148, S-cecb234a3206, S-d2752f2d6c22, S-d6227f9723b1, S-d6eb10fae754, S-d8f8e61b831b, S-da09135fa657, S-dc8001ed8842, S-de5e3879f7ce, S-df27d92d57a0, S-e0a6e2a8d6f1, S-e2a9bd2f0d14, S-e4ff18df1f16, S-e52ad911c3ac, S-e7171507c7aa, S-e7392f226f69, S-e77ffff40807, S-e8cd22f8b451, S-e9dc5003d109, S-eb54787f3a5f, S-ef628d581794, S-ef6c7832a511, S-f37920514535, S-f41f214e88e8, S-f5d75cc3208b, S-f66da6ee5bfa, S-f79b692d55fc, S-f9c430a954b3, S-fac59fa61d8f, S-fcc1e24ec465, S-fcc50bf0b486, S-fd840029644f, S-ff3fcf31d13d, S-ff8255d1567e]
---

# Wayland protocol map

The map separates advertised globals, server versions, member hooks and policy. It compares pinned sway and scroll source, recorded niri evidence, Exo at Chromium 150, the historical CADE audit and Domicile at its pin. Absence is a disposition.

## Reading the map

- [Coverage map](coverage-map.md): one row per listed protocol or global identity.
- [Protocol rows](rows/protocols.tsv): tabular source for the map.
- [Member rows](rows/protocol-members.tsv): requests, events and enums, including version and dispatch limits.
- [Community site entry](wayland-explorer-entry.md): capture format and release metadata.
- [Topics](topics/README.md): the distinctions that span several protocols.

Historical study versions describe the recorded audit. They are neither client minimum versions nor the versions supplied by Exo at Chromium 150. Negotiated resource versions and version-gated requests are separate from an advertised global ceiling. [inferred]

Domicile advertisement and landing status are independent columns. A source-present row can have unknown landing status when its merged-PR and pinned-documentation pair has not been established. A landed capability does not certify every request or the surrounding shell behavior. [inferred]

## Counts

101 identities are listed. The member inventory contains 881 parent/member records, covering 880 distinct wire declarations, across 60 inventory groups. One image-capture source destroy request appears under two parents. Enum values do not count as separate members. A separate vendor catalog preserves additional pinned schema members. The supplied inventory is not a complete declaration set for every schema release; independently verified additions remain separate. [docs]

| Comparison | Listed identities | Meaning |
|---|---:|---|
| Domicile globals | 34 | Interface identities, including its custom anchor protocol; conditional registrations count once |
| Domicile present identity rows | 38 | Includes seat resources and the capture-source umbrella; these add no globals |
| sway served rows | 58 | Three core seat-resource rows are excluded; some served versions remain unresolved |
| scroll served rows | 58 | Three core seat-resource rows are excluded; some served versions remain unresolved |
| Exo at 150 served rows | 43 | Audited server/output registrations; ChromeOS and narrower build gates remain visible |
| Domicile absent rows | 62 | No registration found in the stated source search |

Counts overlap across compositors and include alternate schema or vendor identities. They are not percentages of Wayland conformance. A client may see a smaller list because rendering, backend or sandbox conditions apply. [inferred]

## Collections

- [core](collections/core.md): 11 identities. Core objects and seat-created input resources.
- [stable](collections/stable.md): 4 identities. Stable protocol specifications outside the core wire schema.
- [staging](collections/staging.md): 30 identities. Staging, legacy unstable and custom schemas, with lifecycle recorded separately.
- [ext](collections/ext.md): 12 identities. Ext-prefixed desktop contracts and historical draft names, with lifecycle recorded per row.
- [wlr](collections/wlr.md): 9 identities. Protocols originating in the wlroots ecosystem.
- [kde](collections/kde.md): 1 identity. KDE protocol compatibility surfaces.
- [zcr](collections/zcr.md): 21 identities. ChromeOS and Exo vendor protocols, including Aura wire surfaces.
- [xdg](collections/xdg.md): 13 identities. Desktop-shell, activation and metadata protocols.

## Historical verdict crosswalk

The six CADE tiers describe that audit’s study groups. Its Exo column includes vendor analogs and version-specific judgments. Tier membership is preserved as a dated disposition; the Exo-at-150 column comes from its own pinned evidence. [docs]

| Historical field | This map |
|---|---|
| Group 1–6 | Dated study grouping, without ranking |
| Exo yes or percentage | Historical claim, including its stated analog or partial coverage |
| Exo no | Historical absence plus the affected client behavior where recorded |
| Unimplemented | A disposition and a client-visible limit, without defect framing |

The audit’s table and prose contain conflicting aggregate totals. They are retained as source limitations rather than reconciled into a new percentage. [docs]

| Historical group | Protocol entries | Exo coverage in the audit headline |
|---|---:|---:|
| Group 1 | 13 | 13 / 13 |
| Group 2 | 7 | 5 / 7 |
| Group 3 | 3 | 3 / 3 |
| Group 4 | 8 | 0 / 8 |
| Group 5 | 4 | 0 / 4 |
| Group 6 | 1 | 0 / 1 |

These are the historical headline figures. The tier-1 activation row itself says no, and several yes entries refer to vendor analogs. The per-protocol historical cells retain those qualifications; the modern Exo column is independent. S-385433e1522c · 2026-04-11 · Historical protocol audit [docs]

## Sources

- [Domicile pinned compositor](https://github.com/cprussin/domicile/tree/996d8b628439a5d9b12204b49c9d472f249666bc/packages/domicile-compositor).
- S-000764dce113 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-00ffae850184 · 2026-11-09 · Protocol implementation and registration evidence.
- S-014c456bb822 · 2026-11-09 · Protocol implementation and registration evidence.
- S-016ff15271ae · 2026-11-09 · Protocol implementation and registration evidence.
- S-04faf44f2e81 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-06ca08859f6e · 2026-11-09 · Pinned Chromium 150 vendor XML contract.
- S-083adea7e7ad · 2026-11-09 · Protocol implementation and registration evidence.
- S-0aa2cc30bb07 · 2026-11-09 · Protocol implementation and registration evidence.
- S-0b6c31dfe6bc · 2026-11-09 · Protocol implementation and registration evidence.
- S-0d6c45f5a62b · 2026-11-09 · Protocol implementation and registration evidence.
- S-1346c57ad82c · 2026-11-09 · gtk4-layer-shell 1.3.0 client-library documentation.
- S-135ecf2b8dea · 2026-11-09 · Pinned Chromium 150 vendor XML contract.
- S-1389e89c68c6 · 2026-11-09 · pointer-warp 1.49 XML contract.
- S-1497ad1e40a4 · 2026-11-09 · Protocol implementation and registration evidence.
- S-14b8d68070ec · 2026-11-09 · Protocol implementation and registration evidence.
- S-1570b41820c4 · 2026-11-09 · Protocol implementation and registration evidence.
- S-18a2678e3352 · 2026-11-09 · color-management 1.49 XML contract.
- S-18ec6dfe35f6 · 2026-11-09 · Protocol implementation and registration evidence.
- S-1b81a86927e0 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-1c210691c0b8 · 2026-11-09 · Protocol implementation and registration evidence.
- S-1d7075319a3c · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-1edc2e41b6da · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-1f5025ab76ad · 2026-11-09 · Protocol implementation and registration evidence.
- S-229f7c4d9b14 · 2026-11-09 · Protocol implementation and registration evidence.
- S-2822956bdf13 · 2026-11-09 · Protocol implementation and registration evidence.
- S-291dce183eb2 · 2026-11-09 · Smithay data-device manager and default drag callbacks.
- S-2b83da449b94 · 2026-11-09 · Protocol implementation and registration evidence.
- S-2ea0540e0f1d · 2026-04-12 · Protocol implementation and registration evidence.
- S-2f9c9a88b407 · 2026-11-09 · Pinned Chromium 150 vendor XML contract.
- S-3138604e8df0 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-32784ffd9a8d · 2026-11-09 · Idle activity, blanking and inhibitor contract.
- S-33085e8f5a27 · 2026-11-09 · Smithay input-method manager at the dependency pin.
- S-332161a6dfa1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-3674591ca8e6 · 2026-11-09 · xdg-session-management 1.49 XML contract.
- S-37cd947718b7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-385433e1522c · 2026-04-11 · Historical protocol audit.
- S-3acbfb4907e7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-3c995b7a34f5 · 2026-11-09 · Protocol implementation and registration evidence.
- S-3d63fd27af2d · 2026-11-09 · Clipboard storage, selection and browser boundary contract.
- S-3e03b7086707 · 2026-07-16 · Protocol implementation and registration evidence.
- S-3e37be57c91a · 2026-11-09 · Protocol implementation and registration evidence.
- S-3f4edde82525 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-3f524db52413 · 2026-11-09 · Compositor protocol registration and seat construction.
- S-3f5dbd0c9580 · 2026-11-09 · Protocol implementation and registration evidence.
- S-4378aebc583f · 2026-11-06 · Projected protocol members and schema coordinates.
- S-43af2b3af183 · 2026-11-09 · Protocol implementation and registration evidence.
- S-44598db24690 · 2026-11-09 · Protocol implementation and registration evidence.
- S-44be82b2b78e · 2026-10-23 · Protocol implementation and registration evidence.
- S-46587779628d · 2026-11-09 · Protocol implementation and registration evidence.
- S-46f8f6ded930 · 2026-11-09 · Protocol implementation and registration evidence.
- S-472d3a68890f · 2026-11-09 · Protocol implementation and registration evidence.
- S-499a1aa22127 · 2026-11-09 · Niri 25.08 external X11-to-Wayland bridge documentation.
- S-4aa1e405fd79 · 2026-11-09 · Protocol implementation and registration evidence.
- S-4cd685c56e61 · 2026-11-09 · Protocol implementation and registration evidence.
- S-4d83eed81e29 · 2026-11-09 · Protocol implementation and registration evidence.
- S-4fee70ea6452 · 2026-10-29 · Protocol implementation and registration evidence.
- S-507aa315e372 · 2026-11-09 · Protocol implementation and registration evidence.
- S-51292cdb8ce9 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-51497eee422b · 2026-11-09 · Protocol implementation and registration evidence.
- S-534dcadd1369 · 2026-11-09 · Smithay virtual-keyboard handler at the dependency pin.
- S-548e570973fa · 2026-11-09 · Protocol implementation and registration evidence.
- S-5532dadfd152 · 2026-11-09 · Protocol implementation and registration evidence.
- S-594f7c37cd88 · 2026-11-09 · Protocol implementation and registration evidence.
- S-5bb7550e6b56 · 2026-11-09 · Protocol implementation and registration evidence.
- S-5e4ce5d63275 · 2026-10-22 · Summary of a community protocol-page survey.
- S-5e57a72fa9cc · 2026-11-09 · Protocol implementation and registration evidence.
- S-5eb8003b42fd · 2026-11-09 · Protocol implementation and registration evidence.
- S-5f315b39ac7c · 2026-11-09 · Protocol implementation and registration evidence.
- S-5fb9706b471e · 2026-11-09 · Wlr screencopy version registration.
- S-5fcf9ba138c3 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-610c446f4416 · 2026-11-09 · Pinned Chromium 150 vendor XML contract.
- S-654a5c549cb3 · 2026-11-09 · Protocol implementation and registration evidence.
- S-66930fd17085 · 2026-11-09 · Protocol implementation and registration evidence.
- S-66b2ff508daa · 2026-11-09 · Protocol implementation and registration evidence.
- S-67ccf82fd285 · 2026-07-16 · Historical protocol appendix.
- S-687d7a1db0ce · 2026-11-09 · Protocol implementation and registration evidence.
- S-6ba440ed6420 · 2026-11-09 · Protocol implementation and registration evidence.
- S-6fceeb23f0e9 · 2026-11-09 · Pinned Chromium 150 vendor XML contract.
- S-703c0d408b29 · 2026-11-09 · Protocol implementation and registration evidence.
- S-70da4c3a822b · 2026-11-08 · Protocol implementation and registration evidence.
- S-7170fa893ec4 · 2026-11-09 · Protocol implementation and registration evidence.
- S-71c6b5b6fa74 · 2026-11-09 · Protocol implementation and registration evidence.
- S-728ccd41be9e · 2026-11-09 · Protocol implementation and registration evidence.
- S-74f557738f56 · 2026-11-09 · Output profiles, pointer transforms and gamma contract.
- S-77d28cc4fbfe · 2026-11-09 · Protocol implementation and registration evidence.
- S-782b0928e349 · 2026-11-09 · Protocol implementation and registration evidence.
- S-7a89a244a0f8 · 2026-11-09 · Protocol implementation and registration evidence.
- S-7b295c5a7393 · 2026-11-09 · Protocol implementation and registration evidence.
- S-7b31c5bb3be0 · 2026-04-11 · Historical protocol audit.
- S-7cd85cb7ef61 · 2026-11-09 · Protocol implementation and registration evidence.
- S-7f8302736ae8 · 2026-11-09 · Smithay DRM lease handler at the dependency pin.
- S-7fff5f5a9d7c · 2026-11-09 · Protocol implementation and registration evidence.
- S-82b445bd2404 · 2026-11-09 · Protocol implementation and registration evidence.
- S-832c3939e13e · 2026-11-09 · Protocol implementation and registration evidence.
- S-85c7cd8374aa · 2026-11-09 · Protocol implementation and registration evidence.
- S-863c5efcb1a1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-866b9f3f829d · 2026-11-08 · Public input and desktop compatibility history.
- S-88f5036dac2f · undated · Protocol implementation and registration evidence.
- S-8a46216d2a3e · 2026-11-09 · Protocol implementation and registration evidence.
- S-8bf472405b20 · 2026-11-09 · Protocol implementation and registration evidence.
- S-8c6da920d0f9 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-8e9e08836f9d · 2026-11-09 · Protocol implementation and registration evidence.
- S-9081893da4f7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-914380369a95 · 2026-11-09 · Protocol implementation and registration evidence.
- S-9175c1754b40 · 2026-11-09 · Wayland output creation and fractional-scale reporting.
- S-9297edaee809 · 2026-11-09 · Protocol implementation and registration evidence.
- S-92d9911f0704 · 2026-11-09 · Protocol implementation and registration evidence.
- S-9586d9691bef · 2026-11-09 · Protocol implementation and registration evidence.
- S-961f39214831 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-98268a478f8c · 2026-11-09 · Protocol implementation and registration evidence.
- S-9a9f91803d7e · 2026-11-09 · Smithay text-input manager at the dependency pin.
- S-9e7d963ab743 · 2026-11-09 · Protocol implementation and registration evidence.
- S-9ecc381476d7 · 2026-11-09 · Protocol implementation and registration evidence.
- S-a04ed2a57dc2 · 2026-11-03 · Source study of an earlier Domicile release.
- S-a103e36fe4b1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-a19d3732cf8f · 2026-06-11 · Historical protocol audit.
- S-a1fd4d71b416 · 2026-11-09 · Protocol implementation and registration evidence.
- S-a3671b8e50eb · 2026-11-09 · Protocol implementation and registration evidence.
- S-a542cf9c1edb · 2026-07-16 · Protocol implementation and registration evidence.
- S-a6589c3205d1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-a89a584c0175 · 2026-11-09 · Protocol implementation and registration evidence.
- S-a90d2490bf23 · 2026-11-09 · Native gestures, page defaults and shell binding contract.
- S-ae7fe663e21e · 2026-11-09 · Protocol implementation and registration evidence.
- S-af6e4774be6e · 2026-11-09 · Protocol implementation and registration evidence.
- S-b2415d10a041 · 2026-04-07 · XML-reference inventory.
- S-b32e8bae0905 · 2026-11-04 · Protocol implementation and registration evidence.
- S-b4058c3be3bd · 2026-11-09 · Protocol implementation and registration evidence.
- S-b57d7ad8f1ae · 2026-11-09 · Protocol implementation and registration evidence.
- S-b61833f7b205 · 2026-11-09 · Protocol implementation and registration evidence.
- S-b824688e0e5c · 2026-11-08 · color-representation 1.49 XML contract.
- S-b9df7238ba4e · 2026-11-09 · Protocol implementation and registration evidence.
- S-bad77f83f7f6 · 2026-11-09 · Protocol implementation and registration evidence.
- S-bc6dca48e478 · 2026-11-09 · Protocol implementation and registration evidence.
- S-bd39d7cfd9cb · 2026-11-09 · Protocol implementation and registration evidence.
- S-c2596d00b100 · 2026-11-09 · Portal interfaces, direct capture and sandbox boundary.
- S-c34c6ce697a3 · 2026-11-09 · Protocol implementation and registration evidence.
- S-c4bef886de89 · 2026-11-09 · Protocol implementation and registration evidence.
- S-c4fb695b7c41 · 2026-11-09 · Protocol implementation and registration evidence.
- S-c5d3db1884f1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-c5dec4346034 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-c977034319b9 · 2026-11-08 · Pinned Chromium 150 vendor XML contract.
- S-ca1441e8ae06 · 2026-11-09 · Protocol implementation and registration evidence.
- S-ca6aadf58810 · 2026-11-09 · Protocol implementation and registration evidence.
- S-cc860987e54f · 2026-11-09 · Protocol schema inventory.
- S-ccf976691aa5 · 2026-11-09 · Protocol implementation and registration evidence.
- S-ce8dab4e8148 · 2026-11-09 · Protocol implementation and registration evidence.
- S-cecb234a3206 · 2026-11-09 · Protocol implementation and registration evidence.
- S-d2752f2d6c22 · 2026-11-09 · Protocol implementation and registration evidence.
- S-d6227f9723b1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-d6eb10fae754 · 2026-11-09 · Protocol implementation and registration evidence.
- S-d8f8e61b831b · 2026-11-09 · Protocol implementation and registration evidence.
- S-da09135fa657 · 2026-11-09 · Protocol implementation and registration evidence.
- S-dc8001ed8842 · 2026-11-09 · Pinned Chromium 150 vendor XML contract.
- S-de5e3879f7ce · 2026-07-16 · Protocol implementation and registration evidence.
- S-df27d92d57a0 · 2026-11-09 · Pinned Chromium 150 vendor XML contract.
- S-e0a6e2a8d6f1 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e2a9bd2f0d14 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e4ff18df1f16 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e52ad911c3ac · 2026-11-09 · Domicile drag callbacks and selection serving.
- S-e7171507c7aa · 2026-11-09 · Protocol implementation and registration evidence.
- S-e7392f226f69 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e77ffff40807 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e8cd22f8b451 · 2026-11-09 · Protocol implementation and registration evidence.
- S-e9dc5003d109 · 2026-10-06 · Protocol implementation and registration evidence.
- S-eb54787f3a5f · 2026-11-09 · Protocol implementation and registration evidence.
- S-ef628d581794 · 2026-11-09 · Protocol implementation and registration evidence.
- S-ef6c7832a511 · 2026-11-09 · Image-copy and capture-source manager registration.
- S-f37920514535 · 2026-11-09 · Protocol implementation and registration evidence.
- S-f41f214e88e8 · 2026-11-09 · Protocol implementation and registration evidence.
- S-f5d75cc3208b · 2026-11-09 · Xdg fullscreen output and activation request routing.
- S-f66da6ee5bfa · 2026-11-09 · Protocol implementation and registration evidence.
- S-f79b692d55fc · 2026-11-09 · Protocol implementation and registration evidence.
- S-f9c430a954b3 · 2026-11-09 · Per-output gamma-control manager registration.
- S-fac59fa61d8f · 2026-10-22 · Protocol implementation and registration evidence.
- S-fcc1e24ec465 · 2026-11-09 · Protocol implementation and registration evidence.
- S-fcc50bf0b486 · 2026-11-09 · Protocol implementation and registration evidence.
- S-fd840029644f · 2026-10-22 · Protocol implementation and registration evidence.
- S-ff3fcf31d13d · 2026-11-09 · Protocol implementation and registration evidence.
- S-ff8255d1567e · 2026-11-09 · Protocol implementation and registration evidence.
