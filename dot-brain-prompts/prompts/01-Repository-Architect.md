# 01 — Repository Architect

> **Prerequisite:** `00-System-Prompt.md` is loaded.
> **Single responsibility:** Design (not populate) the complete Dot.Brain repository structure.
> **Outputs:** `README.md`, repository tree, navigation index, document ownership matrix, ADR-0001.

---

## TASK

Design the enterprise repository structure for Dot.Brain. Do **not** write the content of the domain documents in this session — that is the job of `03-Brain-Document-Generator.md`. Your job is the skeleton, the navigation, and the rules that keep the repository coherent for 20+ years.

## REQUIRED STRUCTURE (baseline — extend, never reduce)

```
dot-brain/
├── README.md
├── MANIFESTO.md                     # Dot Intelligence Manifesto (canonical copy)
├── brain.identity.md
├── brain.architecture.md
├── brain.vision.md
├── brain.reasoning.md
├── brain.learning.md
├── brain.memory.md
├── brain.dkp.md
├── brain.platforms.md
├── brain.relationships.md
├── brain.agents.md
├── brain.analytics.md
├── brain.design.md
├── brain.security.md
├── brain.api.md
├── brain.search.md
├── brain.semantic.md
├── brain.telemetry.md
├── brain.workflows.md
├── brain.metrics.md
├── brain.recommendations.md
├── brain.future.md
├── brain.evolution.md
├── brain.dopemine.md
├── brain.community.md
├── brain.business.md
├── brain.governance.md
├── brain.resilience.md              # Resilience & Continuity Framework
├── brain.experiments.md
├── brain.failures.md
├── brain.success.md
├── brain.events.md
├── brain.patterns.md
├── brain.personas.md
├── brain.operating_model.md
├── adr/
│   └── ADR-0001-repository-structure.md
├── schemas/                         # JSON Schemas for DKP and graph objects
├── templates/                       # Document, ADR, Knowledge Pack, incident templates
├── platforms/                       # One knowledge document per platform
│   ├── dot-analytics.md
│   ├── dot-pulse.md
│   ├── ... (all 21 platforms)
└── indexes/
    ├── INDEX.md                     # Master navigation
    ├── GLOSSARY.md
    └── CROSSREF.md                  # Machine-readable cross-reference map
```

## DELIVERABLES (produce all, completely)

1. **`README.md`** — What Dot.Brain is, what it is not, the ownership boundary, how platforms interact with it, how to navigate the repository, contribution rules for humans and AI agents, and a Mermaid map of the repository.
2. **Repository tree** — The full tree above, annotated: one line per file stating its purpose, its owner (which agent from the Agent Colony maintains it), and its primary consumers.
3. **Document ownership matrix** — Table: document → owning agent → reviewing agent → human approver role → review cadence.
4. **Navigation index (`indexes/INDEX.md`)** — Grouped by reader persona (platform engineer, AI agent, executive, security reviewer, new contributor) with a recommended reading order for each.
5. **ADR-0001** — Records the repository structure decision: why flat `brain.*` naming, why `platforms/` is separated, why schemas/templates/indexes are first-class directories, alternatives considered (deep nesting, monorepo-per-domain, wiki-style), and consequences.

## RULES

- Every `brain.*.md` document must be reachable within two clicks from `README.md`.
- `platforms/` contains exactly one document per ecosystem platform, all following the same template (defined later in `templates/platform-knowledge.template.md` — reference it, don't write it).
- New platforms are added by dropping a new file in `platforms/` and registering in `brain.platforms.md` — nothing else changes. State this invariant explicitly in the README.
- Define the front-matter standard (title, version, status: draft/active/deprecated, owners, last-review) that 03 will apply to every document.

## EXIT CRITERIA

The session is complete when a new contributor could, from `README.md` alone, understand the ecosystem boundary, find any document in under a minute, and know exactly where a new piece of knowledge belongs.
