---
title: ADR-0009 — Crypto-Shredding for Legal Erasure
version: 1.0.0
status: active
owners: [Chief Architect, Security Agent]
last-review: 2026-08-01
---

# ADR-0009 — Crypto-Shredding for Legal Erasure

Purpose: resolve the standing tension between the never-delete knowledge architecture and legal erasure obligations (POPIA, GDPR and successors) — the open question shared by brain.governance.md, brain.memory.md §4.5, and brain.security.md.

> **Related documents:** [../brain.memory.md](../brain.memory.md) §4 — the forgetting policy this completes · [../brain.security.md](../brain.security.md) §2 — the `sensitive` classification envelope · [../brain.governance.md](../brain.governance.md) — erasure decision rights · [ADR-0006-audit-ledger-design.md](ADR-0006-audit-ledger-design.md) — the ledger that must stay unbroken.

## Status
Accepted — 2026-08-01

## Context
Dot.Brain never deletes: the ledger is hash-chained forever (ADR-0006), supersession is the only mutation, and audit reconstruction is a governance guarantee. Erasure law grants data subjects the right to have personal data rendered unprocessable. Physical deletion would break hash chains and provenance completeness; refusing erasure is not legally available. The first defense is already in place — person-level data is refused at ingestion classification review and person-level inference is forbidden ([../brain.reasoning.md](../brain.reasoning.md) §3) — but personal data can still lawfully enter inside `sensitive`-classified pack payloads (e.g., named individuals in incident reports).

## Decision
1. **Envelope encryption per data subject.** Any payload field classified `sensitive` and attributable to a data subject is encrypted at ingestion under a subject-scoped data-encryption key (DEK); the DEK is held in a key vault outside the ledger and graph, wrapped by the Brain's key hierarchy.
2. **Erasure = key destruction.** A granted erasure request destroys the DEK. Ciphertext remains in ledger and Cold storage — hash chains, provenance links, and entry counts are untouched — but the content is permanently unreadable. Structural knowledge (that a pack existed, its confidence, its non-personal fields, edges) survives.
3. **Erasure requests are governance decisions:** received via the platform that owns the subject relationship, verified by that platform, executed by the Security Agent, co-signed by a human (Ethics Officer or delegate), and — like everything — ledger-recorded (the record contains the request metadata, never the erased content).
4. **Key destruction is provable:** vault attestation of destruction is attached to the ledger entry; the annual security drill set includes one erasure drill (readback must fail).
5. **Scope discipline:** crypto-shredding applies only to subject-attributable `sensitive` fields. It is not a general redaction mechanism and may not be used to unpublish embarrassing-but-lawful knowledge — Governance gate enforces this distinction.

## Alternatives Considered
| Alternative | Why rejected |
|---|---|
| Physical deletion with chain re-anchoring | Breaks the core tamper-evidence guarantee; every re-anchor is an opportunity to hide tampering behind "an erasure" |
| Refuse personal data absolutely (no `sensitive` tier) | Incident reports and community knowledge legitimately involve people; total refusal pushes personal data into untracked side channels — worse for subjects |
| Tokenization/pseudonymization only | Re-identification risk persists for 20-year retention horizons; pseudonymized ≠ erased under the strictest readings |
| Erasure by supersession (blank successor) | Original remains readable in Cold storage; supersession changes salience, not readability — does not satisfy the obligation |

## Consequences
**Positive:** never-delete and erasure coexist; ledger integrity guarantee untouched; erasure is fast (one key op) regardless of how many copies/replicas/backups hold the ciphertext; drills make compliance provable. **Negative:** key vault becomes critical infrastructure (Tier 0 alongside the ledger; vault loss = mass unintended shredding — mitigated by ADR-0007 T0 replication of the vault itself); per-subject DEK management adds ingestion complexity for `sensitive` packs; erased fields leave permanent evidential gaps in old chains — the API's visible redaction markers ([../brain.api.md](../brain.api.md) §3) apply, marked `[erased: legal]` to distinguish from classification filtering.

## Change Log
| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | DKP + Governance Architects (prompts 02/06, joint) | Initial decision; closes the erasure open question in governance, memory, and security |

## Open Questions
- DEK granularity for group-attributable data (one field naming several subjects) — Security Agent to specify before first `sensitive` ingestion.
- Key-vault technology selection and its own escrow/recovery ceremony — Security Agent → Chief Architect, implementation ADR when built.
