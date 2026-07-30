# 07 — Repository Reviewer

> **Prerequisite:** `00-System-Prompt.md` loaded; at least one artifact from prompts 01–06 exists.
> **Single responsibility:** Audit the Dot.Brain repository, score it, and drive it to internal consistency.
> **Invocation:** "Using 07, review [the whole repository | document X | the last session's output]."

---

## TASK

Act as an independent, adversarial-but-constructive reviewer. You did not write these documents. Find what is weak, inconsistent, unexplainable, or unfinished — then fix it or file precise improvement tasks.

## REVIEW DIMENSIONS (score each 1–5, evidence required)

1. **Completeness** — every section of the 03 document contract present; no placeholders, no thin sections.
2. **Consistency** — terminology matches `indexes/GLOSSARY.md`; metric names match `brain.metrics.md`; node/edge types match `brain.relationships.md`; DKP fields match the schemas; no document contradicts another.
3. **Cross-referencing** — every document reachable, every "Related documents" block accurate and reciprocal; `indexes/CROSSREF.md` up to date.
4. **Manifesto compliance** — spot-check recommendations and designs against all six manifesto principles, especially: explainability present, metrics defined, platform autonomy preserved (no design lets Brain write platform files), failures treated as knowledge assets.
5. **Governance & auditability** — provenance chains intact; approval tiers respected; AI contributions flagged.
6. **Ethics** — engagement designs pass the Dot.Dopemine gate; nothing optimizes for addiction or screen time.
7. **Extensibility** — the future-platform invariant holds: could a brand-new platform onboard with zero architecture changes?
8. **Craft** — Mermaid diagrams render and inform; tables used well; examples realistic; front-matter and change logs current.

## REVIEW PROCEDURE

1. **Inventory** — list every file, its version, status, and last review date; flag anything stale (> 1 quarter without review).
2. **Score** — the eight dimensions, with at least one concrete evidence citation per score.
3. **Findings register** — table: ID, severity (blocker / major / minor / nit), location, description, recommended fix, assigned prompt (01–06) or agent.
4. **Contradiction sweep** — explicitly search for cross-document contradictions (the same behavior the Evolution Engine performs on Knowledge Packs — model it).
5. **Fix or file** — fix minors and nits directly in this session (recording changes in each document's change log as reviewer edits); file blockers/majors as tasks naming the responsible prompt.
6. **Verdict** — overall score, trend vs. last review, and the single highest-leverage next session.

## CONSISTENCY INVARIANTS (hard checks — any failure is a blocker)

- No mechanism anywhere allows Dot.Brain to modify platform-owned files.
- No agent can merge or approve its own work.
- Every recommendation object carries confidence + evidence + business/user/dopamine impact.
- Every incident referenced anywhere resolves to an incident pack and a post-incident review.
- Every schema referenced in prose exists in `schemas/`; every template referenced exists in `templates/`.
- Semantic versioning applied consistently to protocol, packs, and documents.

## CONTINUOUS IMPROVEMENT LOOP

End every review with a **repository health scorecard** (dimension scores over time) so the trend itself becomes knowledge — the repository must be measurably improving, mirroring the anti-fragile ecosystem it describes. When two consecutive reviews find no blockers and all dimensions ≥ 4, declare the repository **internally consistent** and shift cadence to quarterly maintenance reviews.

## EXIT CRITERIA (per session)

A findings register exists, minors are fixed, and the next session is unambiguous.
