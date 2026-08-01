---
title: Dot.Billing — Platform Knowledge
version: 1.0.1
status: active
owners: [Billing Platform Lead, Finance Agent, Registry Agent]
platform-id: dot-billing
dkp-version: 1.0.0
integration-status: publishing
last-review: 2026-08-01
---

# Dot.Billing

> **Platform-owned source:** [Dot.Billing's wiki.md](https://github.com/sakhilebhayi/Dot.Billing/blob/main/wiki.md) — the platform's own knowledge home. This document is Dot.Brain's ingested view; the wiki is authoritative for what the platform actually is.

## 1. Purpose & Business Domain

Payments and subscriptions — settlement of marketplace orders, recurring subscription billing, payout scheduling to merchants and producers, and dunning. Owns the settlement domain: money movement records and their timing. Financial products and credit belong to Dot.Finance; matching and orders to Dot.Emall (§6). Billing's knowledge is the most sensitive in the value chain — its aggregation configuration (§7) is stricter than the ecosystem default and is this document's registry-gap closure.

## 2. Entities Owned

| Entity | Graph node type | Natural key | Notes |
|---|---|---|---|
| Billing account | `entity:site` | `dot:node:finance:account:<id>` | Tenant root (merchant/subscriber) |
| Settlement | `entity:process` | settlement ID | Joins Emall order ID — the chain's handoff key |
| Payout | `entity:process` | account + cycle | Producer/merchant disbursement |
| Subscription | `entity:asset` | account + plan | Recurring billing lifecycle |
| Settlement-latency observation | `observation` | corridor + window | Aggregate only, per §7 |
| Dunning outcome | `outcome` | account + case | Recovery vs. churn ground truth |

## 3. Events Emitted

| Event | Trigger | Consumers | Frequency |
|---|---|---|---|
| `finance.settlement.completed/failed` | Settlement close | Brain ingestion, Dot.Emall, Dot.Analytics | ~10³/day |
| `finance.payout.scheduled/released` | Payout cycle | Brain, Dot.Farms/merchants (via platform UX) | daily cycles |
| `finance.subscription.renewed/lapsed` | Subscription lifecycle | Brain, Dot.Analytics | ~10²/day |
| `finance.dunning.opened/closed` | Recovery case | Brain (aggregate only) | low |

## 4. Knowledge Packs Published

| Payload type | Cadence | Example pack ID |
|---|---|---|
| observation (settlement-latency/payout aggregates) | daily batch | `dkp:billing:obs:2026-07-20:0027` |
| insight (settlement-pattern findings) | per finding | `dkp:billing:ins:2026-06-15:0002` |
| outcome (recommendation verification) | per verified recommendation | `dkp:billing:out:2026-07-31:0001` |
| incident (settlement failures, payout delays) | per incident | `dkp:billing:inc:2026-03-08:0001` |

All Billing packs default to classification `restricted` (money-movement patterns are competitively and criminally sensitive); insights suitable for wider reuse are re-published as `ecosystem` derivatives after the Security gate confirms the §7 floors held.

## 5. Intelligence Consumed

| Recommendation type | Metric expected to move | Baseline |
|---|---|---|
| Settlement-corridor routing | `finance.settlement_latency_p95` | 2026 H1, per corridor |
| Payout-cycle optimization (seasonal producers — the Farms harvest cashflow case) | `finance.payout_delay_p50` | 2026 wet season |
| Dunning-approach selection | `finance.dunning_recovery_rate` | 2026 H1 |

## 6. Cross-Platform Relationships & the Settlement-Latency Seam

```mermaid
flowchart LR
    E[Dot.Emall order fulfilled] -->|order ID handoff| B[Dot.Billing settlement]
    B -->|payout| F[Dot.Farms / merchants]
    B -->|settled-revenue aggregates| A[Dot.Analytics]
    B <-->|credit/financial products| FI[Dot.Finance]
    B -->|subscription billing| ALL[All subscribing platforms]
```

**Settlement-latency seam (named by the Emall round-trip):** the chain metric `order fulfilled → settlement completed` crosses the Emall/Billing boundary. Canonical statement: the seam metric is `finance.settlement_latency_p95`, owned *here* (Billing owns the clock from fulfilment event receipt); Emall's obligation is emitting `commerce.order.fulfilled` within its own event contract. A chain-level view (harvest → listing → order → settlement → payout) is an Analytics product, assembled from each platform's owned metrics — no platform owns another's segment.

## 7. Tenancy Model & Aggregation-Floor Configuration (registry gap closed)

Tenant key = billing account ID; topics `finance.<tenant>.<event>`. Floors — stricter than the ecosystem default, in two tiers:

| Data class | Floor | Rationale |
|---|---|---|
| Settlement/payout aggregates | **n ≥ 50 distinct accounts** per corridor × window | Money-movement patterns de-anonymize at smaller n; intersection-attack rule applies with the wider margin |
| Dunning/recovery aggregates | **n ≥ 100 distinct accounts**, quarterly windows only | Financial-distress signals are `sensitive`-adjacent; smallest publishable cell deliberately coarse |
| Subscription lifecycle aggregates | n ≥ 20 (ecosystem default) | Plan-level churn is low-sensitivity |

Floors are manifest-declared (below) so validation enforces them at ingestion, not by reviewer memory.

## 8. Dopamine Surface

Shares: on-time payout reliability (platform's own performance, outcome-anchored). Explicitly withheld: everything user-facing — payment streaks, spend milestones, dunning-pressure nudges. Billing is the one platform whose engagement mechanics could do direct financial harm; its dopamine surface is minimal by policy, not by omission.

## 9. Active Recommendations

Maintained by the Registry Agent. Current: payout-cycle optimization for seasonal producers `open` (expiry 2026-08-30); settlement-corridor routing `verified` — see §13.

## 10. Incident History Summary

One incident pack (2026-03): payout batch delayed by a corridor outage — F-INFRA; lesson: corridor health checks moved ahead of batch commit, propagated to Finance's payment-adjacent workflows. Consumed: Central's alert-precision tuning pattern for its own corridor monitors.

## 11. Domain Metrics (registered per brain.metrics.md §4.8)

| ID | Type | Definition |
|---|---|---|
| `finance.settlement_latency_p95` | duration | Fulfilment event receipt to settlement completed, p95 per corridor — the seam metric |
| `finance.payout_delay_p50` | duration | Settlement completed to payout released, median |
| `finance.dunning_recovery_rate` | ratio | Recovered cases / opened cases, quarterly |

## 12. Manifest (platform.dkp.json example)

```json
{
  "platform_id": "dot-billing",
  "dkp_version": "1.0.0",
  "signing_key_ref": "vault://keys/dot-billing/dkp-signing/v1",
  "publishes": ["observation", "insight", "outcome", "incident"],
  "subscribes": ["settlement-routing", "payout-cycle-optimization", "dunning-approach"],
  "schemas": { "knowledge-pack": "1.0.0", "metric": "1.0.0" },
  "default_classification": "restricted",
  "tenancy": {
    "key": "account_id",
    "aggregation_floor": 20,
    "floor_overrides": [
      { "data_class": "settlement", "floor": 50 },
      { "data_class": "dunning", "floor": 100, "min_window": "quarter" }
    ]
  }
}
```

## 13. Worked round-trip

The value chain's third link, closed:

1. **Pack:** `dkp:billing:obs:2026-07-20:0027` — settlement-latency aggregates, Northern Cape corridor, 4 weeks, 61 distinct accounts (≥ 50 floor holds); signed, `restricted`.
2. **Validation → graph:** corridor-latency nodes; `OBSERVED_WITH` edge between harvest-season order surges (Emall's demand packs — cross-corroboration ×1.10) and settlement queue depth at 0.69.
3. **PR back:** settlement-corridor routing — pre-scale corridor capacity in the harvest-peak window the Farms/Emall chain already predicts; confidence 0.80, impact `finance.settlement_latency_p95` −20% predicted, guard `finance.payout_delay_p50` flat, expiry 30 days.
4. **Outcome:** `dkp:billing:out:2026-07-31:0001` verifies −24% settlement latency; a producer-facing effect lands one link upstream — Farms' `agriculture.produce_time_to_market_p50` improves without Farms changing anything. Three links verified; the chain-level Analytics product (§6) can now be assembled from owned segments.

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Platform Integrator (prompt 05, AI) | Initial integration package: settlement-domain ownership, seam-metric canonical statement, two-tier aggregation-floor configuration (registry gap closed, manifest-enforced), restricted-by-default classification, minimal dopamine surface by policy, 3 domain metrics, worked round-trip |

| 1.0.1 | 2026-08-01 | Repository Steward Agent | Linked to Dot.Billing's own wiki.md (platform repo) as the platform-owned source of truth |

## Open Questions

| Question | Owner → Approver |
|---|---|
| `restricted`-to-`ecosystem` derivative re-publication: does the Security gate need a standing checklist for Billing derivatives, or per-pack review at current volumes? | Security Agent → Security Officer |
| Dunning floor (n ≥ 100, quarterly) — validate against POPIA/GDPR guidance once the legal-identity open question (brain.identity.md) resolves | Security Agent → Security Officer |
