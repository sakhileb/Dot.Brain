---
title: Platform Knowledge Document Template
version: 1.0.0
status: active
owners: [Chief Knowledge Engineer, Registry Agent]
last-review: 2026-08-01
---

# Platform Knowledge Document Template

Purpose: the mandatory structure for every `platforms/<platform>.md`. One document per platform, identical structure — so agents and humans can compare any two platforms section-by-section.

> **Related documents:** [../brain.platforms.md](../brain.platforms.md) · [../brain.dkp.md](../brain.dkp.md) · [../schemas/platform-manifest.schema.json](../schemas/platform-manifest.schema.json)

---

```markdown
---
title: Dot.<Platform> — Platform Knowledge
version: 1.0.0
status: draft | active | deprecated
owners: [<Platform owner role>, <Domain agent>, Registry Agent]
platform-id: dot-<platform>
dkp-version: <semver>
integration-status: registered | publishing | full-loop
last-review: YYYY-MM-DD
---

# Dot.<Platform>

## 1. Purpose & Business Domain
One paragraph: what the platform does, for whom, and the business domain it owns.

## 2. Entities Owned
Table: Entity → graph node type → natural key → notes. Every entity the platform
is the source of truth for, mapped to brain.relationships.md node types.

## 3. Events Emitted
Table: event name → trigger → consumers → frequency estimate.
Mapped to the taxonomy in brain.events.md.

## 4. Knowledge Packs Published
Table: payload type → cadence → example pack ID. Which DKP payload types this
platform publishes and how often.

## 5. Intelligence Consumed
Table: recommendation type subscribed → metric it is expected to move → baseline.
What the platform wants back from Dot.Brain.

## 6. Cross-Platform Relationships
Explicit edges to other platforms with direction and meaning
(e.g., value chains, operational loops). Mermaid flowchart required.

## 7. Tenancy Model
How multi-tenant data stays isolated through ingestion and reasoning:
tenant key, topic naming, aggregation floors.

## 8. Dopamine Surface
Which user progress/achievement signals it shares with Dot.Dopemine, and the
ethical constraints (allowed metric classes; prohibited list applies).

## 9. Active Recommendations
Live table maintained by the Registry Agent: open PRs from Dot.Brain, status, expiry.

## 10. Incident History Summary
Rolling summary of incident packs published and lessons contributed/received.

## Change Log
| Version | Date | Author | Change |

## Open Questions
```

---

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Platform Integrator (prompt 05) | Initial template |

## Open Questions
- None.
