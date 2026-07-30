# 02 — DKP Architect (Dot Knowledge Protocol)

> **Prerequisite:** `00-System-Prompt.md` loaded; repository structure from `01` exists.
> **Single responsibility:** Design the complete Dot Knowledge Protocol — the communication contract between every platform and Dot.Brain.
> **Outputs:** `brain.dkp.md`, `schemas/knowledge-pack.schema.json`, supporting schemas, `templates/knowledge-pack.template.md`, worked examples, ADRs.

---

## TASK

Create **DKP** — the versioned, signed, auditable protocol through which platforms publish knowledge and Dot.Brain returns intelligence. DKP is the only way knowledge crosses the ownership boundary. Design it so a platform that does not exist yet can implement it from the spec alone.

## PROTOCOL SCOPE (all sections mandatory)

### 1. Knowledge Packs
- Definition, lifecycle (draft → published → ingested → validated → related → superseded/deprecated), envelope structure, payload types.
- Payload types must cover: **Entity Models, Event Models, Workflow Models, Metrics, Insights, Recommendations, Documents, Discussions, Learning History, Incident Reports** (see §9).

### 2. Versioning & Compatibility
- Semantic versioning for both the protocol (`dkp_version`) and each pack (`pack_version`).
- Compatibility rules: what a MAJOR/MINOR/PATCH change means for consumers; how Dot.Brain handles packs from older protocol versions; deprecation strategy with sunset windows; future-extension mechanism (`x-` extension fields that never break validation).

### 3. Validation & Trust
- Schema validation, referential validation (do referenced entities exist in the graph?), semantic validation (contradiction detection).
- **Trust Scores** per publishing platform and per contributor (human or AI), computed from historical accuracy, review outcomes, and incident involvement.
- **Knowledge Confidence** per assertion (0.00–1.00) with the formula inputs documented: source trust × validation results × corroboration count × age decay.

### 4. Relationships & Impact
- How packs declare **Knowledge Relationships** (relates-to, supersedes, contradicts, depends-on, derived-from) referencing graph node IDs.
- Required impact declarations on every Recommendation: **Business Impact, User Impact, Dopamine Impact** (ethical engagement impact per Dot.Dopemine principles) — each with a metric, baseline, and target.

### 5. Provenance & Signatures
- **Knowledge Provenance**: full chain from raw source → transformation steps → publishing contributor.
- **Digital Signatures**: platform signing keys, AI contributor keys, human contributor keys; signature verification on ingestion; key rotation.
- **AI Contributors vs Human Contributors**: both are first-class, both identified, both accountable; AI contributions flagged as such permanently.

### 6. Approval Workflow & Review
- Review process: automated checks → agent peer review → human approval tiers (by impact level).
- **Merge Strategy** for accepted knowledge; **Conflict Resolution** when packs contradict (evidence weighing, confidence comparison, escalation to human arbiter, recording of the resolution as new knowledge).
- **Change Logs**: every pack and every resolution appends to an immutable log.

### 7. Pull Request Contract
- The exact structure of a PR Dot.Brain sends back to a platform: summary, rationale, evidence links, confidence, impact estimates, rollback note. Platforms decide; silence ≠ consent.

### 8. Transport & Registry
- Publish/subscribe model, platform registration manifest (`platform.dkp.json`), authentication, idempotency, retry semantics, rate limits, multi-tenant isolation guarantees.

### 9. Incident Knowledge (anti-fragility)
- **DKP Incident Reporting**: a dedicated pack type for incidents, outages, security events, failed experiments, rollbacks, and disasters — with fields for detection, impact, timeline, root cause, corrective actions, and verified lessons.
- **Knowledge propagation**: how verified lessons fan out as advisory PRs to all platforms that share the vulnerable pattern.

## DELIVERABLES

1. `brain.dkp.md` — full spec with Mermaid sequence diagrams for: pack publication, ingestion & validation, PR round-trip, conflict resolution, incident propagation.
2. `schemas/knowledge-pack.schema.json` + schemas for entity, event, workflow, metric, insight, recommendation, incident payloads.
3. `templates/knowledge-pack.template.md` and a **fully worked example**: a real-looking pack from Dot.Mines (e.g., haul-truck cycle-time insight) and the resulting Dot.Brain PR back to Dot.Central.
4. ADRs for: signature scheme choice, versioning policy, conflict-resolution model.

## EXIT CRITERIA

A platform team, given only `brain.dkp.md` and the schemas, could implement a compliant publisher and consumer without asking a single question.
