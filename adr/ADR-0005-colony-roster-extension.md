---
title: ADR-0005 — Colony Roster Extension
version: 1.0.0
status: active
owners: [Chief AI Engineer, Governance Agent]
last-review: 2026-08-01
---

# ADR-0005 — Adding Registry and Repository Steward Agents to the Colony

Purpose: record the decision to extend the prompt-04 minimum roster with two agents.

> **Related documents:** [../brain.agents.md](../brain.agents.md) · [../README.md](../README.md) ownership matrix

## Status
Accepted — 2026-08-01

## Context
The prompt-04 minimum roster (22 agents) leaves two ownership-matrix rows from prompt 01 without an owning agent: (a) platform registration/manifests/`brain.platforms.md`, and (b) README/indexes/templates. Assigning these to existing agents would violate separation of concerns: Architecture Agent duties are protocol-spec work, not onboarding operations; Documentation Agent enforces the document contract, not repository structure.

## Decision
Add two agents:
1. **Registry Agent** — owns platform onboarding, manifest validation, `brain.platforms.md`, `brain.future.md`; guardian of the "one file + one row, nothing else" invariant.
2. **Repository Steward Agent** — owns README, indexes, templates; guardian of the two-click navigation property.

## Alternatives Considered
| Alternative | Why rejected |
|---|---|
| Fold registry duties into DKP/Architecture agents | Onboarding is operational, continuous work; spec agents would deprioritize it, and the invariant needs a dedicated guardian |
| Fold stewardship into Documentation Agent | Contract enforcement (per-document) and navigation integrity (cross-document) have different failure modes and rubrics |
| Leave rows human-owned only | Contradicts colony design: every document row needs an owning agent for continuous maintenance |

## Consequences
**Positive:** complete ownership-matrix coverage; both 20-year invariants (registration, navigation) have accountable guardians. **Negative:** roster grows to 24 — mitigated by the rule that adding agents is charter + row only, no architecture change.

## Change Log
| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Agent Colony Architect (prompt 04) | Initial decision |

## Open Questions
- None.
