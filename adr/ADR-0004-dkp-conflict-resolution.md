---
title: ADR-0004 — DKP Conflict-Resolution Model
version: 1.0.0
status: active
owners: [Chief AI Engineer, Governance Agent]
last-review: 2026-08-01
---

# ADR-0004 — Conflict Resolution: Evidence-Weighed Auto-Resolution with Human Arbiter Escalation

Purpose: record how contradicting knowledge is resolved in the Dot.Brain graph.

> **Related documents:** [../brain.dkp.md](../brain.dkp.md) §6.2 · [../brain.governance.md](../brain.governance.md)

## Status
Accepted — 2026-08-01

## Context
Packs will contradict — across platforms, across time, between AI and human contributors. Silent overwrites violate the manifesto ("approved knowledge is never overwritten"); resolving everything by hand does not scale; resolving everything automatically is unexplainable and unsafe.

## Decision
1. Contradictions detected at semantic validation open a **conflict case** — the incoming pack is held in `contradicts` state, never rejected outright and never merged silently.
2. **Automated resolution** when evidence is decisive: compare recomputed confidences (which already fold in trust, corroboration, recency). If Δconfidence ≥ **0.20**, the higher-confidence assertion wins; the loser is marked `superseded`, retained with full provenance.
3. **Human arbiter escalation** when indecisive (Δ < 0.20), when either assertion is security/ethics-flagged, or when the conflict is human-vs-AI with the AI side winning. The arbiter receives a complete evidence dossier.
4. **Every resolution is new knowledge**: a resolution node `derived-from` both assertions, recording method, evidence weights, actor, and reasoning — appended to the immutable change log.

## Alternatives Considered
| Alternative | Why rejected |
|---|---|
| Last-writer-wins | Silent knowledge loss; trivially gameable; violates auditability |
| Always-human resolution | Does not scale to graph size; humans burn out on trivial conflicts |
| Fully automated (no escalation) | Unexplainable edge cases; unacceptable for security/ethics-flagged knowledge |
| Voting among agents | Confidence already aggregates evidence; voting adds collusion risk without new information |

## Consequences
**Positive:** scalable (most conflicts decisive), safe (humans own the hard cases), anti-fragile (resolutions themselves become searchable precedent). **Negative:** the 0.20 threshold is a tunable governance parameter — it must be reviewed against arbiter workload quarterly; held packs add latency for contradicting knowledge (accepted: correctness over speed).

## Change Log
| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | DKP Architect (prompt 02) | Initial decision |

## Open Questions
- Precedent reuse: should resolved conflict cases auto-resolve future identical contradiction patterns? (Proposed yes, with confidence cap 0.90; needs governance sign-off.)
