---
title: ADR-0008 — Blameless Review Policy
version: 1.0.0
status: active
owners: [SRE Lead, Governance Agent]
last-review: 2026-08-01
---

# ADR-0008 — Blameless Post-Incident Review Policy

Purpose: record the decision that all incident learning is blameless, and why this is enforced structurally rather than culturally.

> **Related documents:** [../brain.resilience.md](../brain.resilience.md) Capability 5 · [../templates/post-incident-review.template.md](../templates/post-incident-review.template.md)

## Status
Accepted — 2026-08-01

## Context
The anti-fragility loop depends entirely on honest incident data. If reporting an incident can harm the reporter, incidents get hidden, timelines get sanitized, and the learning engine trains on fiction. This applies equally to human contributors and AI agents (whose trust scores create analogous disincentives).

## Decision
1. **Root causes are systems, conditions, and process gaps — never individuals.** The PIR template's blameless attestation (§8) is mandatory; a PIR naming a person as a cause is returned unaccepted.
2. **Facilitator independence:** the PIR facilitator is never in the incident's chain of command.
3. **AI symmetry with a distinction:** an agent's *incident involvement* affects its trust score (accountability per DKP §3.2), but *reporting or surfacing* an incident never does — self-reported incidents carry a trust-neutral flag. Detection is rewarded; involvement is accounted; concealment is the only offense.
4. **No disciplinary use:** ledger incident data may not be cited in individual performance processes; Governance Agent audits for violations.

## Alternatives Considered
| Alternative | Why rejected |
|---|---|
| Accountability-based reviews (name responsible parties) | Empirically suppresses reporting; poisons the knowledge base the whole ecosystem learns from |
| Blameless for humans, punitive for AI agents | Agents would learn to avoid detection-heavy work; symmetry keeps colony incentives honest |
| Cultural norm without structural enforcement | Norms decay over 20 years; templates, attestations, and audits do not |

## Consequences
**Positive:** honest timelines, faster reporting (MTTD includes willingness-to-report), lessons grounded in real causal chains; agents keep claiming risky-but-valuable detection work. **Negative:** genuine negligence must be handled in separate, non-ledger processes — an accepted cost, kept strictly firewalled from the learning loop.

## Change Log
| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Governance & Resilience Architect (prompt 06) | Initial decision |

## Open Questions
- None.
