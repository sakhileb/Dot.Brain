---
title: ADR-0010 — Domain-Agent Roster Extension (F-06 Assignments)
version: 1.0.0
status: active
owners: [Chief AI Engineer, Governance Agent]
last-review: 2026-08-01
---

# ADR-0010 — Adding People, Logistics, Delivery, and Extension Agents; Narrowing the Marketplace Agent

Purpose: record the decision to extend the colony roster from 24 to 28 agents, driven by evidence from the F-06 platform integration sessions.

> **Related documents:** [../brain.agents.md](../brain.agents.md) §1.1 · [../platforms/dot-agents.md](../platforms/dot-agents.md) §7.1 · [ADR-0005](ADR-0005-colony-roster-extension.md)

## Status
Accepted — 2026-08-01

## Context
Platform integration (prompt 05) found four platforms whose registry-placeholder agents were wrong fits: Dot.HR needs an agent whose hard limits are PII-tier enforcement, not business opportunity; Dot.Ehail's location-privacy regime doesn't belong in a commerce agent; Dot.Projects/Dot.Tasks form a shared delivery boundary no existing agent owns; Dot.Plug's third-party certification duties conflict with the Marketplace Agent's commerce mission (an agent curating seller success should not also police extension publishers). Until chartered, all four ran under `runc:provisional` — blocking full-autonomy operation (finding F-07-01).

## Decision
1. Add four agents with charters: **People** (dot-hr), **Logistics** (dot-ehail), **Delivery** (dot-projects, dot-tasks), **Extension** (dot-plug).
2. Narrow the **Marketplace Agent** to Dot.Emall + Dot.Auction (fixed-price and mechanism commerce), removing Dot.Plug and Dot.Ehail from its scope.

## Alternatives Considered
| Alternative | Why rejected |
|---|---|
| Stretch Marketplace to cover all four | Conflicting hard limits (commerce growth vs. PII/certification policing); single agent becomes a review bottleneck across five platforms |
| One generic "Operations" agent for HR/Ehail/Projects/Tasks | Domain rubrics differ fundamentally (PII tiers vs. geo-privacy vs. schedule calibration); a generic rubric enforces none well |
| Keep `runc:provisional` indefinitely | Defeats colony autonomy; every routine recommendation escalates to humans permanently |

## Consequences
**Positive:** F-07-01/F-07-04 cleared; each platform's hard limits live in a charter whose reviewer can enforce them; Marketplace's mission is coherent again. **Negative:** roster grows to 28 — mitigated by the standing rule (charter + row, no architecture change); four new agents start at trust 0.50 with all output human-reviewed until earned.

## Change Log
| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Agent Colony Architect (prompt 04, AI) | Initial decision |

## Open Questions
- None.
