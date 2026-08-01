---
title: ADR-0001 — Dot.Brain Repository Structure
version: 1.0.0
status: active
owners: [Chief Architect, Architecture Agent]
last-review: 2026-08-01
---

# ADR-0001 — Repository Structure

Purpose: record the decision that defines the Dot.Brain repository layout — flat `brain.*` core documents, first-class `platforms/`, `schemas/`, `templates/`, `adr/`, and `indexes/` directories.

> **Related documents:** [../README.md](../README.md) · [../indexes/INDEX.md](../indexes/INDEX.md)

---

## Status

Accepted — 2026-08-01

## Context

Dot.Brain must remain navigable and coherent for 20+ years, across 21+ platforms, maintained jointly by humans and an Agent Colony. Requirements: every document reachable within two clicks of `README.md`; new platforms integrate with zero architectural change; knowledge is versioned, superseded (never overwritten), and machine-traversable; contribution boundaries must be enforceable per-document.

## Decision

1. **Flat `brain.<domain>.md` naming at the root** for all core domain documents.
   - Guarantees the two-click rule trivially; the filename *is* the address.
   - `brain.` prefix gives a stable, greppable, sortable namespace and unambiguous cross-references from platforms and agents.
   - Flatness removes taxonomy debates: domains evolve by adding a file, never by re-nesting (renames break 20-year provenance chains).
2. **`platforms/` as a separated directory** with exactly one document per platform.
   - Platform docs are a different *class*: template-driven, co-owned with the platform, added/removed as the ecosystem changes. Separation keeps the core stable while the periphery grows.
   - Enforces the registration invariant: new platform = one new file + one registry row in `brain.platforms.md`, nothing else.
3. **`schemas/`, `templates/`, `indexes/`, `adr/` as first-class directories.**
   - `schemas/` — machine-validated contracts (DKP, graph objects) must be tool-addressable at a fixed path, independent of prose.
   - `templates/` — quality bar enforcement: uniform structure is a rule, not a convention.
   - `indexes/` — navigation and the machine-readable `CROSSREF.md` are artifacts of the graph itself and must be regenerable without touching content documents.
   - `adr/` — an append-only decision ledger is the backbone of "everything is explainable."

## Alternatives Considered

| Alternative | Why rejected |
|---|---|
| **Deep nesting** (`core/intelligence/reasoning.md`, …) | Taxonomies rot; re-nesting breaks links and provenance; violates two-click rule as tree grows; agents and humans disagree on categorization. |
| **Monorepo-per-domain** (separate repo per domain/platform) | Destroys the single knowledge graph; cross-references become network calls; governance and audit fragment across repos; contradicts "one brain." |
| **Wiki-style** (hosted wiki, DB-backed pages) | No PR-based governance, weak versioning/provenance, poor schema validation, not agent-friendly; conflicts with "everything auditable" and supersede-never-overwrite. |
| **Prefix-free flat root** (`reasoning.md`) | Namespace collisions with tooling files; loses the instantly-recognizable brain namespace in cross-repo references. |

## Consequences

**Positive:** stable addresses for 20+ years; trivial two-click navigation; platform onboarding is O(1); per-file ownership maps cleanly to the Agent Colony and CODEOWNERS-style enforcement; machine agents can traverse via `indexes/CROSSREF.md` without parsing prose.

**Negative / accepted costs:** root directory is wide (~35 files) — mitigated by `indexes/INDEX.md` persona navigation; flat naming forbids sub-domain files, so growing domains must split into new sibling documents (`brain.reasoning.md` → add `brain.inference.md`) with an ADR recording the split.

**Follow-ups:** prompt 03 populates documents using the front-matter standard in [../README.md](../README.md); prompt 04 confirms the Agent Colony roster referenced in the ownership matrix.

---

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Repository Architect (prompt 01) | Initial decision record |

## Open Questions

- Trigger criteria for splitting an oversized domain document into siblings (proposed: > ~2,500 lines or > 3 distinct owner agents) — to be ratified in a future ADR.
