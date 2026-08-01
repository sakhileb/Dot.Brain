---
title: Resilience Scorecard Template
version: 1.0.0
status: active
owners: [SRE Lead, Resilience Agent]
last-review: 2026-08-01
---

# Resilience Scorecard Template

Purpose: the quarterly per-platform (and Dot.Brain-itself) scorecard defined by [brain.resilience.md](../brain.resilience.md) Capability 6. Produced by the Resilience Agent; reviewed at the quarterly governance audit.

> **Related documents:** [../brain.resilience.md](../brain.resilience.md) · [../brain.governance.md](../brain.governance.md) §7

---

```markdown
# Resilience Scorecard — <platform | dot-brain> — <YYYY-Qn>

## Headline
| Indicator | This quarter | Last quarter | Trend | Target |
|---|---|---|---|---|
| Repeat-incident rate (propagated patterns) | | | ↓/→/↑ | declining |
| MTTD p50 / p95 (min) | | | | declining |
| MTTR p50 / p95 (min) | | | | declining |
| Lesson-adoption rate (advisories accepted) | | | | ≥ 50% |
| RTO/RPO breaches | | | | 0 |

## Drills & chaos
| Exercise | Tier | Pass criteria | Result | Findings → incidents |
|---|---|---|---|---|

## Incidents this quarter
| ID | Sev | Kind | Lessons verified | Propagated to |
|---|---|---|---|---|

## Open corrective actions past due
| Incident | Action | Owner | Days overdue |
|---|---|---|---|

## Anti-fragility verdict (Evolution Agent)
One paragraph: is this platform measurably harder to hurt than last quarter?
Evidence required. "Yes" without declining repeat-incident rate is not accepted.

## Signatures
- Resilience Agent (author) · SRE Lead (approver) · Governance Agent (audit record)
```

---

## Change Log
| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Governance & Resilience Architect (prompt 06) | Initial template |

## Open Questions
- None.
