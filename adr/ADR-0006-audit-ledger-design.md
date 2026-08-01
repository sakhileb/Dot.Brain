---
title: ADR-0006 — Audit Ledger Design
version: 1.0.0
status: active
owners: [Chief Architect, Governance Agent]
last-review: 2026-08-01
---

# ADR-0006 — Hash-Chained Append-Only Audit Ledger

Purpose: record the design of the immutable audit trail required by brain.governance.md §2.

> **Related documents:** [../brain.governance.md](../brain.governance.md) · [../brain.dkp.md](../brain.dkp.md) §6.3

## Status
Accepted — 2026-08-01

## Context
Every pack transition, PR decision, review, override, merge, conflict resolution, and drill result must be verifiable forever, by auditors and agents, with tamper-evidence — across a 20-year horizon and multiple storage generations.

## Decision
- **Append-only log, hash-chained:** each entry embeds `prev_hash` (SHA-256 of the previous canonicalized entry); periodic checkpoint hashes are co-signed by the Governance Agent key and a human-held offline key.
- **Entries are JCS-canonicalized JSON** (same canonicalization as DKP signatures — one canonical form ecosystem-wide).
- **Retention: forever; supersession, never deletion.** Corrections are new entries referencing the corrected one.
- **Tier-0 service** (RTO 1 h, RPO 0, sync replication per ADR-0007).
- Quarterly full-chain integrity verification; any break is a sev2 incident.

## Alternatives Considered
| Alternative | Why rejected |
|---|---|
| Public/permissioned blockchain | Operational and cost overhead without added trust: all writers are already identified and signed; consensus solves a problem we don't have |
| Plain database audit table | No tamper-evidence; a privileged writer could rewrite history silently |
| Write-once object storage only (WORM) | Immutability without linkage — gaps/deletions between objects are not detectable |
| Signed entries without chaining | Detects forgery of an entry, not removal of one |

## Consequences
**Positive:** tamper-evident, cheap to verify incrementally, same canonical form as DKP, replayable for event-sourced recovery. **Negative:** hash chain serializes writes per shard — mitigated by per-tenant sub-chains with cross-checkpointing; crypto-agility (SHA-256 → successor) requires a documented re-anchoring procedure (open question in brain.governance.md handles the erasure tension).

## Change Log
| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Governance & Resilience Architect (prompt 06) | Initial decision |

## Open Questions
- Re-anchoring procedure for hash-algorithm migration — Architecture Agent to draft before 2028 review.
