---
title: ADR-0007 — RTO/RPO Service Tier Model
version: 1.0.0
status: active
owners: [SRE Lead, Resilience Agent]
last-review: 2026-08-01
---

# ADR-0007 — Four-Tier RTO/RPO Model

Purpose: record the service-tier model and recovery targets in brain.resilience.md Capability 4.

> **Related documents:** [../brain.resilience.md](../brain.resilience.md) · [ADR-0006](ADR-0006-audit-ledger-design.md)

## Status
Accepted — 2026-08-01

## Context
Recovery investment must be proportional to blast radius. The critical architectural fact: **no platform's critical path depends on a live Dot.Brain** (ownership boundary = BCP by design). Recovery targets therefore protect trust artifacts and knowledge first, intelligence second, convenience last.

## Decision
| Tier | Services | RTO | RPO | Rationale |
|---|---|---|---|---|
| 0 | Audit ledger, key registry | 1 h | 0 | Trust artifacts: losing one entry breaks verifiability forever |
| 1 | Graph store, ingestion, DKP transport | 4 h | 5 min | Knowledge loss is near-irreplaceable; publishers buffer ≥ 4 h |
| 2 | Reasoning, recommendation, search | 12 h | 1 h | Recomputable from tier 1; latency tolerable |
| 3 | Dashboards, reports, colony drafts | 48 h | 24 h | Convenience surfaces; fully rebuildable |

Tier assignment is reviewed at the quarterly governance audit; new services must declare a tier at design review.

## Alternatives Considered
| Alternative | Why rejected |
|---|---|
| Uniform aggressive targets (everything RPO≈0) | Sync replication everywhere: cost without benefit for recomputable tiers |
| Two tiers (critical / non-critical) | Ledger (RPO 0) and graph (RPO 5 min) genuinely differ; conflating them either overpays or under-protects |
| Per-service bespoke targets | Ungovernable at 20-year scale; drills can't systematically cover unbounded target combinations |

## Consequences
**Positive:** drill program maps directly onto tiers (alternating quarterly coverage); cost concentrates where loss is irreversible; publisher buffering requirement (≥ 4 h) becomes an explicit DKP transport expectation. **Negative:** tier boundaries create cliff incentives (services argue for lower tiers) — mitigated by design-review tier declaration and audit re-checks.

## Change Log
| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Governance & Resilience Architect (prompt 06) | Initial decision |

## Open Questions
- Regional regulatory variants (data-residency) may require per-geography tier-0 placements — Security Agent to assess.
