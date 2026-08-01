---
title: Worked Example — Dot.Mines Haul-Truck Cycle-Time Pack → Dot.Central PR
version: 1.0.0
status: active
owners: [Chief Knowledge Engineer, Architecture Agent]
last-review: 2026-08-01
---

# Worked Example: Full DKP Round-Trip

Purpose: a complete, realistic example of the DKP lifecycle — Dot.Mines publishes a haul-truck cycle-time insight + recommendation; Dot.Brain validates, ingests, and generates a PR to Dot.Central.

> **Related documents:** [knowledge-pack.template.md](knowledge-pack.template.md) · [../brain.dkp.md](../brain.dkp.md)

---

## Part 1 — The pack Dot.Mines publishes

```json
{
  "dkp_version": "1.0.0",
  "pack_id": "dkp:dot-mines:7f3e2a10-9c4b-4d8e-b1a2-5e6f7a8b9c0d",
  "pack_version": "1.0.0",
  "platform": "dot-mines",
  "title": "Haul-truck cycle time increases 12.4% in the first hour after shift change at Kolomela pit",
  "summary": "Fleet telemetry over 90 days shows loaded haul cycle time rises from a 22.6 min baseline to 25.4 min during the first hour after each shift change, driven by dispatch queue rebuilding. Recommending pre-shift dispatch warm-handover in Dot.Central.",
  "created_at": "2026-07-28T06:12:00Z",
  "contributors": [
    { "id": "ai:dot-mines:fleet-analyst-2", "kind": "ai", "display_name": "Mines Fleet Analyst Agent", "key_id": "dm-ai-fa2-2026" },
    { "id": "human:dot-mines:n.mokoena", "kind": "human", "display_name": "N. Mokoena (Mine Planning Engineer)", "key_id": "dm-hu-nm-2026" }
  ],
  "payloads": [
    {
      "payload_type": "metric",
      "body": {
        "metric_name": "haul.cycle_time_loaded",
        "domain": "fleet-operations",
        "definition": "Minutes from shovel load complete to dump complete, loaded legs only, per truck, excluding refuel legs",
        "unit": "minutes",
        "direction_of_good": "down",
        "dimensions": ["pit", "shift", "hour_offset_from_shift_start"],
        "observations": [
          { "timestamp": "2026-07-27T06:00:00Z", "value": 25.4, "dimensions": { "pit": "kolomela-1", "hour_offset_from_shift_start": "0" }, "sample_size": 1180 },
          { "timestamp": "2026-07-27T08:00:00Z", "value": 22.6, "dimensions": { "pit": "kolomela-1", "hour_offset_from_shift_start": "2" }, "sample_size": 1244 }
        ],
        "baseline": 22.6,
        "target": 23.0
      }
    },
    {
      "payload_type": "insight",
      "body": {
        "statement": "Post-shift-change dispatch queue rebuild causes a 12.4% loaded cycle-time increase during hour 0–1 at Kolomela pit 1",
        "domain": "fleet-operations",
        "method": "90-day telemetry regression, shift boundary as intervention variable, controlled for weather and haul distance",
        "evidence": [
          { "kind": "dataset", "reference": "dotmines://kolomela/fleet-telemetry/2026-04-28_2026-07-27", "note": "412,000 cycles" },
          { "kind": "metric", "reference": "haul.cycle_time_loaded" }
        ],
        "scope": "site:kolomela-1; likely generalizes to all pits using cold dispatch handover"
      }
    },
    {
      "payload_type": "recommendation",
      "body": {
        "proposal": "Implement pre-shift dispatch warm-handover in Dot.Central: incoming dispatcher inherits live queue state and truck assignments 15 minutes before shift boundary instead of rebuilding from empty",
        "target_platform": "dot-central",
        "rationale": "Queue rebuild is the dominant contributor (est. 78%) to the hour-0 cycle-time spike; warm handover removes the rebuild entirely",
        "evidence": ["dkp:dot-mines:7f3e2a10-9c4b-4d8e-b1a2-5e6f7a8b9c0d#insight-1"],
        "impact": {
          "business": { "metric": "haul.cycle_time_loaded", "baseline": 25.4, "target": 23.0, "measurement_window": "60d post-merge, hour 0-1 slices" },
          "user": { "metric": "dispatcher.handover_task_time", "baseline": 18, "target": 8, "measurement_window": "30d post-merge" },
          "dopamine": { "metric": "dispatcher.shift_start_confidence_score", "baseline": 3.1, "target": 4.0, "measurement_window": "60d post-merge survey" }
        },
        "rollback": {
          "procedure": "Feature flag central.dispatch.warm_handover=false; queue rebuild path remains intact",
          "blast_radius": "Dispatch module only; no effect on truck control or safety systems",
          "watch_signals": ["dispatch.assignment_error_rate", "haul.cycle_time_loaded", "dispatcher.override_rate"]
        },
        "review_window_days": 30
      }
    }
  ],
  "relationships": [
    { "type": "relates-to", "target": "dot:node:fleet-operations:c2a1e9d4-1111-4222-8333-944445555666", "note": "Kolomela dispatch workflow node" },
    { "type": "derived-from", "target": "dkp:dot-mines:11aa22bb-33cc-44dd-8ee0-ff0011223344", "note": "Prior cycle-time baseline pack" }
  ],
  "provenance": {
    "sources": [
      { "kind": "sensor", "uri": "dotmines://kolomela/fleet-telemetry/2026-04-28_2026-07-27", "observed_at": "2026-07-27T23:59:59Z" }
    ],
    "transformations": [
      { "step": "cycle segmentation", "tool": "mines-telemetry-pipeline", "tool_version": "4.2.1", "parameters": { "exclude": "refuel_legs" }, "actor": "ai:dot-mines:fleet-analyst-2" },
      { "step": "shift-boundary regression", "tool": "mines-analytics", "tool_version": "2.8.0", "parameters": { "controls": ["weather", "haul_distance"] }, "actor": "ai:dot-mines:fleet-analyst-2" },
      { "step": "human review & framing", "tool": "manual", "tool_version": "n/a", "actor": "human:dot-mines:n.mokoena" }
    ],
    "published_by": "human:dot-mines:n.mokoena"
  },
  "confidence": 0.87,
  "signatures": [
    { "key_id": "dot-mines-platform-2026", "algorithm": "ed25519-jcs", "signed_at": "2026-07-28T06:15:02Z", "value": "kX9…(base64)…Qz==" },
    { "key_id": "dm-hu-nm-2026", "algorithm": "ed25519-jcs", "signed_at": "2026-07-28T06:14:40Z", "value": "aB3…(base64)…yW==" }
  ]
}
```

**Ingestion result:** signatures verified; schema/referential/semantic checks pass; confidence recomputed to **0.83** (source_trust 0.91 × validation 1.0 × corroboration 0.9 × age_decay 1.0... publisher's 0.87 retained in provenance). Pack state → `related`. Recommendation is impact-tier **T3** (cross-platform: mines → central) ⇒ agent peer review + Chief Knowledge Engineer approval, then PR generation.

## Part 2 — The PR Dot.Brain opens against Dot.Central

> Opened by `dot-brain[bot]` on `dot-central` repository. Body:

```markdown
---
summary: Add pre-shift dispatch warm-handover to eliminate post-shift-change cycle-time spike
confidence: 0.83
review_window: 30d
source_pack: dkp:dot-mines:7f3e2a10-9c4b-4d8e-b1a2-5e6f7a8b9c0d
impact:
  business: { metric: haul.cycle_time_loaded, baseline: 25.4, target: 23.0, window: 60d post-merge }
  user: { metric: dispatcher.handover_task_time, baseline: 18, target: 8, window: 30d post-merge }
  dopamine: { metric: dispatcher.shift_start_confidence_score, baseline: 3.1, target: 4.0, window: 60d survey }
---

## Summary
Dot.Mines telemetry (90 days, 412k cycles, Kolomela pit 1) shows loaded haul cycle time
rises 12.4% in the first hour after shift change because dispatch queues rebuild from empty.
This PR proposes a warm-handover mode: the incoming dispatcher inherits live queue state
15 minutes before the shift boundary.

## Rationale
Queue rebuild is estimated to cause 78% of the hour-0 spike. Warm handover removes the
rebuild entirely. Reasoning chain: dot:node:fleet-operations:c2a1e9d4… → insight-1 → this proposal.

## Evidence
- Pack dkp:dot-mines:7f3e2a10… (insight + metric, confidence 0.83)
- Dataset: dotmines://kolomela/fleet-telemetry/2026-04-28_2026-07-27

## Confidence
0.83 = source_trust 0.91 × validation 1.00 × corroboration 0.90 × age_decay 1.00

## Impact
See front-matter. Dopamine impact targets dispatcher confidence at shift start (ethical:
mastery/confidence metric), not engagement time.

## Rollback
Feature flag `central.dispatch.warm_handover=false`. Blast radius: dispatch module only.
Watch: dispatch.assignment_error_rate, haul.cycle_time_loaded, dispatcher.override_rate.

## Decision
Dot.Central owns this decision. This PR expires 2026-08-27 (30d). Silence = no-decision,
recorded and learned from. Approving merges the workflow change spec into central's backlog docs.
```

**Round-trip closure:** whatever Dot.Central decides (accept / reject / expire), the outcome is ingested back into the graph, updates Dot.Mines' and the Fleet Analyst Agent's trust scores, and tunes future recommendation thresholds.

---

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | DKP Architect (prompt 02) | Initial worked example |

## Open Questions

- None.
