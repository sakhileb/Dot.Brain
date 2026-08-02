---
title: Dot Ecosystem — Appendix
version: 1.2.0
status: active
owners: [Sakhile Bhayi]
last-review: 2026-08-01
---

# Appendix — Platform Registry, Assets, Glossary, Pointers

Purpose: a single reference page for the facts a new reader of this `os/` document set needs on hand — the real platform list with status and repo, where the logo assets live, the handful of ecosystem-specific terms this doc set uses without re-defining each time, and where to go for more depth. This is reference material, not doctrine; where it conflicts with a canonical `brain.*.md` file or `indexes/GLOSSARY.md`, those win.

> **Related documents:** [01-Executive-Vision.md](01-Executive-Vision.md) · [../indexes/GLOSSARY.md](../indexes/GLOSSARY.md) — canonical ecosystem-wide glossary, this appendix only summarizes · [../indexes/INDEX.md](../indexes/INDEX.md) — full navigation index · [../brain.platforms.md](../brain.platforms.md) — the platform registry this table is drawn from · [../platforms/](../platforms/) — one knowledge document per platform

---

## 1. The platform list (as of 2026-08-01)

Repo URL pattern is `github.com/sakhilebhayi/<name>` unless a naming discrepancy is flagged (noted below — the registry entry and the actual repo name disagree for two platforms).

| Platform | Repo | Status | Notes |
|---|---|---|---|
| Dot.Billing | `github.com/sakhilebhayi/Dot.Billing` | Real code | Payments and subscriptions |
| Dot.Ehail | `github.com/sakhilebhayi/Dot.Ehail` | Real code | E-hailing entrepreneurship platform |
| Dot.Auction | `github.com/sakhilebhayi/Dot.Auction` | Real code | Auction marketplace; real buyer bidding |
| Dot.Agents | `github.com/sakhilebhayi/Dot.Agents` | Real code | AI agent orchestration + governance stack |
| Dot.Emall | `github.com/sakhilebhayi/Dot.Emall` | Real code | Marketplace; real cart/checkout |
| Dot.Notify | `github.com/sakhilebhayi/Dot.Notify` | Real code | Universal notification platform |
| Dot.Pulse | `github.com/sakhilebhayi/Dot.Pulse` | Real code | Social & community platform |
| Dot.Analytics | `github.com/sakhilebhayi/Dot.Analytics` | Real code | Business intelligence and analytics |
| Dot.Mines | `github.com/sakhilebhayi/mines` | Real code | **Naming discrepancy:** repo is `mines`, not `Dot.Mines`; registry entry is `dot-mines` |
| Dot.Projects | `github.com/sakhilebhayi/Dot.Projects` | Real code | Project management |
| Dot.Tasks | `github.com/sakhilebhayi/Dot.Tasks` | Real code | Task management |
| Dot.Finance | `github.com/sakhilebhayi/Dot.Finance` | Real code | Personal finance tracker — **not** the regulatory/financial-products platform originally envisioned; that scope was retracted, see [platforms/dot-finance.md](../platforms/dot-finance.md) |
| Dot.Charts | `github.com/sakhilebhayi/ChartSense` | Real code | **Naming discrepancy:** repo is `ChartSense`, not `Dot.Charts`; early-stage trading-chart tool with honestly-labeled demo output, not the full SMC/ICT platform described in `brain.*` docs |
| Dot.Central | `github.com/sakhilebhayi/Dot.Central` | Real code | AI-agent command center; mining-dispatch domain scaffold added this session — domain mismatch vs. original brief flagged in [platforms/dot-central.md](../platforms/dot-central.md) |
| Dot.Design | `github.com/sakhilebhayi/Dot.Design` | Real code | AI canvas design tool; token/component-library scaffold added this session — domain mismatch vs. original brief flagged in [platforms/dot-design.md](../platforms/dot-design.md) |
| Dot.Plug | `github.com/sakhilebhayi/Dot.Plug` | Real code (hand-authored, unverified) | Developer marketplace / extension framework — Jetstream shell copied from Dot.Billing, Extension/Installation domain built from scratch |
| Dot.Farms | `github.com/sakhilebhayi/Dot.Farms` | Real code (hand-authored, unverified) | Agriculture ERP — Farm/Field/Crop/CropCycle/HarvestRecord domain built from scratch |
| Dot.HR | `github.com/sakhilebhayi/Dot.HR` | Real code (hand-authored, unverified) | Human Resource platform — Employee/Position/LeaveRequest domain built from scratch; the role-gating gap noted in earlier revisions of this table was closed in a second pass, see [13-Engineering-State.md](13-Engineering-State.md) §3 |
| Dot.Dopemine | `github.com/sakhilebhayi/Dot.Dopemine` | Real code (hand-authored, unverified) | Engagement intelligence engine — Mechanic catalog with the ethics constraint enforced at three structural layers |
| Dot.Memory | `github.com/sakhilebhayi/Dot.Memory` | Real code (hand-authored, unverified) | Long-term semantic memory — storage/retrieval telemetry domain with "store without reading" enforced at the schema level |
| Dot.Files | `github.com/sakhilebhayi/Dot.Files` | Real code (pre-existing, integrated 2026-08-02) | File & folder manager — discovered via InfoDot's platform registry, not originally tracked; audited and registered same day |
| Dot.Docs | `github.com/sakhilebhayi/Dot.docs` | Real code (pre-existing, integrated 2026-08-02) | Real-time collaborative document/wiki platform |
| Dot.Forms | `github.com/sakhilebhayi/Dot.Forms` | Real code (pre-existing, integrated 2026-08-02) | Form builder & submission dispatch |
| Dot.Sheet | `github.com/sakhilebhayi/Dot.Sheet` | Real code (pre-existing, integrated 2026-08-02) | Spreadsheet platform |
| Dot.Engage | `github.com/sakhilebhayi/Dot.Engage` | Real code (pre-existing, integrated 2026-08-02) | Contract sharing, chat & video signing — **domain mismatch:** registry's `campaign` icon assumed a marketing product; real code is contracts/chat/video-signing, see [platforms/dot-engage.md](../platforms/dot-engage.md) |
| Dot.Press | `github.com/sakhilebhayi/Dot.Press` | Real code (pre-existing, integrated 2026-08-02) | Slide-deck design tool — **domain mismatch:** registry's `newspaper` icon assumed a newsroom product; real code is a Canva-style design tool on Inertia+Vue (not Livewire, unlike every sibling platform), see [platforms/dot-press.md](../platforms/dot-press.md) |
| Dot.Tutor | `github.com/sakhilebhayi/Dot.Tutor` | Real code (pre-existing, integrated 2026-08-02) | Tutoring marketplace — schema + dashboard only, no booking UI yet |
| Dot.Brain | (this repo) | Fully speced, not a SaaS app | ~35 `brain.*.md` docs, 28 platform knowledge docs, 12 ADRs — the knowledge/reasoning layer, not a product platform |

**Count:** 28 platforms now have real code — the original 20 (15 extending an already-installed Jetstream app, 5 hand-authored from an empty scaffold), plus 7 more (Dot.Files, Dot.Docs, Dot.Forms, Dot.Sheet, Dot.Engage, Dot.Press, Dot.Tutor) discovered via InfoDot's own platform registry and integrated on 2026-08-02 — turned out to be real, substantially pre-built platforms nobody had tracked, not empty scaffolds. Plus Dot.Brain itself (the knowledge layer, structurally different from the other 28). "~20 platforms" in earlier parts of this doc set is now understated; the ecosystem is 28 product platforms. **None of the 28 has been executed against a real PHP/Postgres environment** — see [13-Engineering-State.md](13-Engineering-State.md) §4.

## 2. Logo assets

Location: `/Users/sakhilebhayi/Downloads/Dot.logos/` (local to the owner's machine, not committed to this repo). Every platform used in the engineering loop this session has a resolved source file — 13 as individually-named files, 8 as unsorted numbered sheets (`dot.logos.png` through `dot.logos9.png`, one platform each) that were identified by visual inspection and used directly, not left ambiguous:

| Platform | Source file |
|---|---|
| Dot.Analytics | `dot.analytics.png` |
| Dot.Billing | `dot.billing.png` |
| Dot.Brain | `dot.brain.png` |
| Dot.Charts (repo: ChartSense) | `dot.charts.png` |
| Dot.Dopemine | `dot.dopamine.png` (note: filename spelled "dopamine," platform is "Dopemine" — an existing spelling divergence, not a typo introduced here) |
| Dot.Farms | `dot.farms.png` |
| Dot.Finance | `dot.finance.png` |
| Dot.HR | `dot.hr.png` |
| Dot.Memory | `dot.memory.png` |
| Dot.Mines (repo: mines) | `dot.mines.png` |
| Dot.Notify | `dot.notify.png` |
| Dot.Plug | `dot.plug.png` |
| Dot.Pulse | `dot.pulse.png` |
| Dot.Emall | `dot.logos.png` |
| Dot.Ehail | `dot.logos2.png` |
| Dot.Agents | `dot.logos3.png` |
| Dot.Auction | `dot.logos4.png` |
| Dot.Central | `dot.logos5.png` |
| Dot.Projects | `dot.logos6.png` |
| Dot.Tasks | `dot.logos7.png` |
| Dot.Design | `dot.logos8.png` |

`dot.logos9.png` is a duplicate of `dot.finance.png`. `dot.logos10.png` and the root `logo.png` are the owner's personal brand mark, not a platform logo — excluded from this table. Every platform above already has its real logo committed into its own repo's `public/images/logo.png` (or equivalent) as of this session's engineering-loop passes; this table exists so a future pass can re-derive the mapping without re-inspecting each image.

## 3. Glossary (ecosystem-specific terms used in this `os/` doc set)

Full definitions live in [../indexes/GLOSSARY.md](../indexes/GLOSSARY.md); this table is a quick-reference subset for terms this doc set leans on.

| Term | Short definition | Canonical source |
|---|---|---|
| **DKP (Dot Knowledge Pack)** | The signed, versioned, schema-validated envelope through which a platform publishes knowledge to Dot.Brain; the only way anything crosses the platform/brain boundary. Not yet implemented on any live platform — see [03-Business-Automation.md](03-Business-Automation.md). | [../brain.dkp.md](../brain.dkp.md) |
| **Knowledge Pack** | Synonym for DKP instance — one published, immutable unit (draft → published → ingested → validated → related → superseded/deprecated). | [../brain.dkp.md](../brain.dkp.md) §1 |
| **wiki.md vs. platforms/*.md** | `wiki.md` lives inside each platform's own repo and is platform-owned, authoritative ground truth. `platforms/<name>.md` in this repo is Dot.Brain's *ingested view* of that platform — it can drift or be flagged as mismatched (see Dot.Central, Dot.Design, Dot.Charts entries in §1) and the wiki always wins on conflict. | [../brain.platforms.md](../brain.platforms.md) |
| **The platform-loop** | The audit → fix → test → document → commit cycle run against one platform per pass. This session ran it in **bounded, single-platform form** — not the unbounded, ecosystem-wide, continuously-running version some `brain.*.md` docs describe as a future target. See [14-Credit-Optimization.md](14-Credit-Optimization.md). | [../brain.workflows.md](../brain.workflows.md) |
| **MVP scaffold** | A platform repository with a Jetstream Teams skeleton (auth, routing) and a bounded, first-pass domain layer — not a fully-featured product. All 20 platforms are at roughly this maturity level today; none has been executed against a real environment. | this appendix, §1 |
| **Domain mismatch (flagged)** | A documented discrepancy between what a `platforms/*.md` doc describes and what the platform's real repo actually implements (e.g. Dot.Central's mining-dispatch description vs. its real AI command-center codebase) — recorded, not silently corrected, pending human reconciliation. | [../platforms/dot-central.md](../platforms/dot-central.md), [../platforms/dot-design.md](../platforms/dot-design.md) |
| **Naming discrepancy (flagged)** | A documented mismatch between a platform's registry name and its actual GitHub repo name (Dot.Mines → `mines`, Dot.Charts → `ChartSense`). | [../platforms/dot-mines.md](../platforms/dot-mines.md), [../platforms/dot-charts.md](../platforms/dot-charts.md) |
| **Superseded scope note** | The documented pattern for recording that an earlier, larger platform vision was retracted in favor of what the real code turned out to be (used for Dot.Finance). Old content is preserved in git history, not carried forward as current fact. | [../platforms/dot-finance.md](../platforms/dot-finance.md) |

## 4. Where to go for more

- **[../indexes/GLOSSARY.md](../indexes/GLOSSARY.md)** — the canonical, ecosystem-wide glossary (one definition per term, with a canonical source cited for each). This appendix's §3 is a narrow excerpt, not a replacement.
- **[../indexes/INDEX.md](../indexes/INDEX.md)** — persona-based navigation across the entire Dot.Brain repository (Platform Engineer, AI Agent, Executive, Security Reviewer, New Contributor reading orders).
- **[../indexes/CROSSREF.md](../indexes/CROSSREF.md)** — the cross-reference graph between documents.
- **[../brain.platforms.md](../brain.platforms.md)** — the platform registry itself, one level below this appendix's summary table.
- **[../platforms/](../platforms/)** — one knowledge document per platform, each with a "Platform-owned source" link to that platform's real `wiki.md`.

---

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Owner + AI (os/ document set, session 1) | Initial appendix; platform list, logo location, and glossary excerpt verified against this repo and the logos folder at write time |
| 1.1.0 | 2026-08-01 | Sakhile Bhayi | Updated all 5 previously-empty platforms to "Real code (hand-authored, unverified)" now that they're built and pushed; corrected the MVP-scaffold glossary entry accordingly |
| 1.2.0 | 2026-08-02 | Sakhile Bhayi | Added 7 platforms (Dot.Files, Dot.Docs, Dot.Forms, Dot.Sheet, Dot.Engage, Dot.Press, Dot.Tutor) discovered via InfoDot's own platform registry — real, substantially pre-built repos nobody had tracked. Ecosystem count moves from 20 to 28 product platforms. Corrected Dot.HR's row (its role-gating gap was closed, not still open). |

## Open Questions

| Question | Owner |
|---|---|
| Should the logo folder be reorganized (one confirmed file per platform) and its confirmed contents re-verified before this appendix is treated as a complete asset inventory? | Sakhile Bhayi |
| Should the two naming discrepancies (Dot.Mines → `mines`, Dot.Charts → `ChartSense`) be resolved by renaming the repos or by formally recording the alias in the registry, per the open flag in each platform's own `platforms/*.md`? | Sakhile Bhayi |
| Now that Dot.Central and Dot.Design have domain-mismatch flags against their `platforms/*.md` docs, should those docs be rewritten to match the real codebases (same treatment as Dot.Finance's superseded-scope note), or does that belong to a different session's scope? | Sakhile Bhayi |
