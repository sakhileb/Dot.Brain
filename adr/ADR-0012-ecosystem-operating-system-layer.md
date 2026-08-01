---
title: ADR-0012 — The os/ Ecosystem Operating System Layer
version: 1.0.0
status: active
owners: [Chief Architect, Sakhile Bhayi]
last-review: 2026-08-01
---

# ADR-0012 — The os/ Ecosystem Operating System Layer

Purpose: record the decision to add `os/` — a 20-document-plus-appendix set covering ecosystem-wide operating doctrine — as a new top-level directory in Dot.Brain, distinct from and above the existing `brain.*.md` technical specification layer.

> **Related documents:** [../README.md](../README.md) · [../os/README.md](../os/README.md) · [ADR-0001-repository-structure.md](ADR-0001-repository-structure.md) — the original repository-structure decision this extends

---

## Status

Accepted — 2026-08-01

## Context

By this point in the ecosystem's build-out, 15 of ~20 Dot platforms had real Laravel/Jetstream codebases, each audited through a bounded engineering-loop pass that found and fixed real bugs (authorization gaps, a checkout race condition, confidentiality-by-convention instead of enforcement, missing tenant scoping). Dot.Brain's own `brain.*.md` library — 35+ documents — specifies the knowledge/reasoning layer in deep technical detail, but nothing in the repository stated the ecosystem-wide *operating* doctrine one level up: how the whole portfolio is actually engineered, secured, owned, and run day to day by a solo operator using AI agents as the primary engineering workforce. That gap became acute once real, comparable findings existed across 15 platforms and needed a place to be synthesized rather than left scattered across 15 separate `wiki.md` files.

## Decision

1. **`os/` as a new top-level directory, sibling to `brain.*.md`, not nested inside it.**
   - `brain.*.md` answers "how does Dot.Brain work." `os/` answers "how does the whole ~20-platform ecosystem work, including Dot.Brain as one platform among them." Nesting one inside the other would misrepresent the relationship — Dot.Brain is a *subject* of `os/`'s doctrine, not its container.
2. **21 numbered documents (01–20, plus Appendix) grounded in this session's real, verifiable state, not aspirational fiction.**
   - Every document was required to cite real findings (the 6 named security fixes, the 5 empty scaffolds, the 3 domain-mismatch platforms) rather than invent a fictional larger organization. Where a document extrapolates beyond verified fact (e.g. proposed cadences in `09-Business-Operating-System.md`), it says so explicitly rather than presenting extrapolation as established policy.
3. **Explicit cross-linking down to `brain.*.md`, never re-deriving it.**
   - Where `os/` and `brain.*.md` cover adjacent ground (security, resilience, governance), the `os/` document states ecosystem-wide doctrine and links to the `brain.*.md` document covering Dot.Brain's own internal mechanics, rather than duplicating detail that would drift out of sync.
4. **Written in parallel by five scoped background agents, then reconciled by a single reviewer pass.**
   - Given the volume (21 documents), the set was authored by five parallel agents grouped by theme, each explicitly forbidden from running git commands to avoid concurrent-commit conflicts in a shared working tree. A single reconciliation pass then fixed cross-agent gaps: a missed file (`15-MEGA-v2.md`), placeholder findings in `13-Engineering-State.md` that a later agent correctly declined to fabricate, an unresolved logo-mapping ambiguity in `Appendix.md`, and a `README.md` written with provisional guessed titles before the other 20 files existed. This pattern — parallel scoped authorship, single-owner reconciliation before commit — is itself now documented as ecosystem doctrine in [os/08-Agent-System.md](../os/08-Agent-System.md).

## Alternatives Considered

| Alternative | Why rejected |
|---|---|
| **Fold this content into existing `brain.*.md` files** (e.g. extend `brain.operating_model.md`, `brain.governance.md`) | Those documents specify Dot.Brain's own internal governance/operating model, not the ecosystem's. Conflating the two would make `brain.operating_model.md` simultaneously about Dot.Brain-the-platform's internal operation and about the 20-platform ecosystem's operation — an ownership-boundary violation the same class as the one ADR-0001 already rejected for platform docs. |
| **A single mega-document instead of 21 chapters** | The zip file this set originated from (`Dot_Ecosystem_Operating_System_Structure.zip`) already established a 21-chapter numbered structure with distinct topics; splitting keeps each document independently reviewable and updatable (e.g. `13-Engineering-State.md` changes weekly, `01-Executive-Vision.md` should not) — the same flat-file-per-domain reasoning ADR-0001 already applied to `brain.*.md`. |
| **Leave the zip's stub files as-is, write content only when a document is actually needed** | Rejected per explicit owner instruction: "let's create content for each .md file before running them" — the stub set was judged to have no usable content, and filling it out before use was preferred over ad hoc partial authoring. |

## Consequences

**Positive:** the ecosystem now has one place to answer "how do things actually work here" without reading 35+ `brain.*.md` files or 15 platform `wiki.md` files individually; the engineering-loop pattern, the security-finding classes, and the DKP-adoption gap are each stated once and cross-linked from everywhere else; the scoring model (`15-MEGA-v2.md`) gives a single comparable number per platform for triage.

**Negative / accepted costs:** `os/` is a second place documentation can go stale alongside `brain.*.md` and each platform's `wiki.md` — three layers now need to stay reconciled. `13-Engineering-State.md` and `15-MEGA-v2.md` are explicitly living documents that will drift if not updated as platforms progress; both carry an in-document staleness warning as the mitigation, not a technical enforcement mechanism.

**Follow-ups:** `13-Engineering-State.md` and `15-MEGA-v2.md` should be updated after every future platform pass. The five platforms currently scored `S=1` in `15-MEGA-v2.md` (Agents, Pulse, Analytics, Central, Design) are flagged there as needing a dedicated deeper security pass, separate from the general engineering loop.

---

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Repository Steward Agent (human-directed) | Initial decision record for the os/ layer |

## Open Questions

- Should `os/` documents eventually get their own review-cadence entries in the root `README.md`'s Document Ownership Matrix, the way every `brain.*.md` file does? Not yet added — deferred to the next matrix update.
