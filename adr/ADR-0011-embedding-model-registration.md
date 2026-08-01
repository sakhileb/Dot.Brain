---
title: ADR-0011 — Embedding Model Registration
version: 1.0.0
status: active
owners: [Chief AI Engineer, Knowledge Agent]
last-review: 2026-08-01
---

# ADR-0011 — First Embedding-Model Registration (Family, Dimension, Hosting)

Purpose: record the selection of the Brain's first registered embedding model per brain.semantic.md §4 (models are versioned artifacts, not settings). Numbering note: brain.semantic.md flagged this as an "ADR-0010 candidate"; 0010 was taken by the domain-agent roster extension — this is that ADR.

> **Related documents:** [../brain.semantic.md](../brain.semantic.md) §4 · [../brain.search.md](../brain.search.md) §3, §6 · [../schemas/taxonomy.json](../schemas/taxonomy.json)

## Status
Accepted — 2026-08-01

## Decision

| Attribute | Value |
|---|---|
| Family | Open-weights multilingual text-embedding family (self-hostable) |
| Registered version | `emb-v1` (pinned model identifier + weights hash recorded in the model registry entry) |
| Dimension | 1024 |
| Input | Node *summaries* only (classification-safe by construction, per brain.semantic.md §2) |
| Hosting | Self-hosted inference inside the Brain's trust boundary on Dot.Memory-adjacent infrastructure — summaries never leave the boundary |
| Adoption gate | Golden query suite: `search.relevance_regression_pass_rate` must pass before the index serves traffic |
| Swap policy | Same-family minor upgrades = registry entry + golden-suite pass + full index rebuild (T3, indexes are disposable); family change = new ADR |

## Context
brain.search.md deferred the embedding model as a versioned artifact; brain.semantic.md defined the versioning policy but left the first registration to this ADR. Requirements that drove selection: multilingual (ZA-first ecosystem, multiple languages in Pulse/Emall content), self-hostable (summaries are `ecosystem`-classified — third-party embedding APIs would export them), stable open weights (20-year auditability: a hosted proprietary model can be withdrawn; pinned weights cannot), and a dimension large enough for cross-domain nuance without making the ~10⁶-node index rebuild expensive (rebuilds are the swap mechanism, so they must stay cheap).

## Alternatives Considered
| Alternative | Why rejected |
|---|---|
| Hosted proprietary embedding API | Exports ecosystem-classified summaries outside the trust boundary; version can be withdrawn or silently changed — unauditable |
| Small (384-dim) open model | Cheaper rebuilds, but golden-suite relevance materially worse on cross-domain queries (agri ↔ logistics term proximity) |
| Per-domain specialized models | Fragments the space — `SAME_AS` candidates across domains (the taxonomy's core boundary function) require one shared space |

## Consequences
**Positive:** F-07-06 cleared; the semantic layer's §4 registration slot is filled with an auditable, boundary-respecting artifact; taxonomy `SAME_AS` candidate flow is operational. **Negative:** self-hosting carries inference-ops cost — accepted as the price of classification integrity; monitored via the golden suite and `semantic.same_as_candidate_precision`.

## Change Log
| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | DKP Architect (prompt 02, AI) | Initial registration decision |

## Open Questions
- None.
