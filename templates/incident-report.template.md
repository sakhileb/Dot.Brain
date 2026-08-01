---
title: Incident Report Template
version: 1.0.0
status: active
owners: [SRE Lead, Resilience Agent]
last-review: 2026-08-01
---

# Incident Report Template

Purpose: the authoring companion for `incident_report` DKP payloads ([schema](../schemas/incident.schema.json)). Complete every section; the pack is machine-validated but this template keeps the content honest.

> **Related documents:** [../brain.resilience.md](../brain.resilience.md) · [post-incident-review.template.md](post-incident-review.template.md)

---

```markdown
# Incident <incident_id> — <one-line title>

- **Kind:** incident | outage | security_event | failed_experiment | rollback | disaster
- **Severity:** sev1–sev4 · **Platform:** dot-<platform> · **Status:** open | mitigated | closed

## Detection
- Detected at (UTC): · Detected by (human/agent/monitor): · Method:
- MTTD (minutes from first fault to detection):

## Impact
- Systems affected: · Users affected (count/estimate):
- Business cost estimate: · Description (plain language):

## Timeline (UTC, append-only)
| At | Event | Actor |
|---|---|---|

## Root Cause
- Statement (the trigger):
- Contributing factors (the conditions — usually ≥ 2):
- Pattern refs (graph node IDs of the vulnerable pattern — REQUIRED for propagation):

## Corrective Actions
| Action | Owner | Due | Status |
|---|---|---|---|

## Lessons
| Lesson | Verified? | Verification evidence |
|---|---|---|
<!-- Unverified lessons stay attached to this incident until proven.
     Only verified lessons propagate (DKP §9.2). -->

## Resolved at (UTC):
```

Rules: blameless language only (name systems and conditions, not people — [ADR-0008](../adr/ADR-0008-blameless-review-policy.md)); timeline is append-only; `pattern_refs` empty ⇒ Resilience Agent will bounce the pack.

---

## Change Log
| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Governance & Resilience Architect (prompt 06) | Initial template |

## Open Questions
- None.
