---
title: Post-Incident Review Template
version: 1.0.0
status: active
owners: [SRE Lead, Resilience Agent]
last-review: 2026-08-01
---

# Blameless Post-Incident Review (PIR) Template

Purpose: the structured, blameless review held within 5 business days (sev1/sev2) or 10 (sev3) of incident mitigation. Its output feeds verified lessons into the learning pipeline ([brain.resilience.md](../brain.resilience.md) Capability 5).

> **Related documents:** [incident-report.template.md](incident-report.template.md) · [../adr/ADR-0008-blameless-review-policy.md](../adr/ADR-0008-blameless-review-policy.md)

---

```markdown
# PIR — Incident <incident_id>

- **Held:** <date> · **Facilitator (human):** · **Attendees (roles):**
- **Resilience Agent evidence bundle:** <link to timeline, telemetry, ledger entries>

## 1. What happened (narrative, 1 paragraph, plain language)

## 2. Causal chain (AI-proposed, human-confirmed)
| # | Cause / condition | Evidence | Confirmed by |
|---|---|---|---|
<!-- Reasoning Engine proposes; the room confirms, corrects, or rejects each link. -->

## 3. What went well
<!-- Detection that worked, containment that held, BCP behavior. Symmetry rule:
     successes here feed brain.success.md just as failures feed brain.failures.md. -->

## 4. What was lucky
<!-- Near-miss factors that saved us this time but won't next time. -->

## 5. Detection & response gaps
| Gap | Proposed fix | Owner | Due |
|---|---|---|---|

## 6. Candidate lessons
| Lesson | Verification plan (how we will PROVE it prevents recurrence) | Owner |
|---|---|---|

## 7. Prevention updates
<!-- Monitors, chaos scenarios, review gates, onboarding checklists to change. -->

## 8. Blameless attestation
Facilitator confirms: no individual named as a cause; all causes are systems,
conditions, or process gaps. ☐
```

Rules: the facilitator is never in the incident's chain of command; the Reasoning Engine's causal chain is input, never verdict; section 8 is mandatory — a PIR naming a person as root cause is returned unaccepted.

---

## Change Log
| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Governance & Resilience Architect (prompt 06) | Initial template |

## Open Questions
- None.
