---
title: Platform Registry & Universal Onboarding
version: 1.0.19
status: active
owners: [Chief Knowledge Engineer, Registry Agent]
reviewing-agent: Knowledge Agent
human-approver: Chief Knowledge Engineer
last-review: 2026-08-01
---

# brain.platforms — The Platform Registry

Purpose: the single registration point for every platform in the Dot Ecosystem and the definition of the invariant onboarding path any future platform follows. A platform exists to Dot.Brain if and only if it has a row here and a validated manifest. Read by the Registry Agent (owner), platform teams onboarding, and every agent resolving a `platform` claim.

> **Related documents:**
> - [brain.dkp.md](brain.dkp.md) — the protocol every registered platform speaks; transport & registry rules in §8.
> - [schemas/platform-manifest.schema.json](schemas/platform-manifest.schema.json) — validation contract for `platform.dkp.json`.
> - [templates/platform-knowledge.template.md](templates/platform-knowledge.template.md) — structure of each `platforms/<platform>.md`.
> - [brain.future.md](brain.future.md) — reserved namespaces and extension surface for platforms that don't exist yet.
> - [README.md](README.md) — the registration invariant this document enforces.

---

## 1. The Registration Invariant

> **A new platform joins by adding one manifest, one knowledge document, and one row in this registry. Nothing else in Dot.Brain changes.**
> If a platform cannot onboard this way, file an ADR — the architecture, not the platform, is at fault.

## 2. Platform Registry

Integration status: `registered` (manifest validated) → `publishing` (packs ingested) → `full-loop` (accepting/deciding PRs). Trust baselines start at 0.50 per DKP §3.2.

| Platform ID | Responsibility | Status | DKP | Trust | Domain Agent | Knowledge Doc | Open Gaps |
|---|---|---|---|---|---|---|---|
| dot-brain | Collective intelligence layer (this system) | full-loop | 1.0.0 | — | Colony | [doc](platforms/dot-brain.md) | — |
| dot-memory | Long-term semantic memory | publishing | 1.0.0 | 0.58 | Memory | [doc](platforms/dot-memory.md) | — |
| dot-analytics | Business intelligence & analytics | publishing | 1.0.0 | 0.58 | Data | [doc](platforms/dot-analytics.md) | — |
| dot-pulse | Social & community platform | publishing | 1.0.0 | 0.58 | Community | [doc](platforms/dot-pulse.md) | — |
| dot-plug | Developer marketplace & extensions | publishing | 1.0.0 | 0.58 | Extension | [doc](platforms/dot-plug.md) | — |
| dot-mines | Mining ERP | publishing | 1.0.0 | 0.62 | Mining | [doc](platforms/dot-mines.md) | — (integration package complete; worked pack is the canonical thread) |
| dot-notify | Universal notification platform | publishing | 1.0.0 | 0.58 | Documentation | [doc](platforms/dot-notify.md) | — |
| dot-billing | Payments & subscriptions | publishing | 1.0.0 | 0.58 | Finance | [doc](platforms/dot-billing.md) | — |
| dot-charts | AI-powered trading platform | publishing | 1.0.0 | 0.58 | Trading | [doc](platforms/dot-charts.md) | — |
| dot-farms | Agriculture ERP | publishing | 1.0.0 | 0.58 | Agriculture | [doc](platforms/dot-farms.md) | seasonal scope fields (tracked in doc OQ) |
| dot-hr | Human Resource platform | publishing | 1.0.0 | 0.58 | People | [doc](platforms/dot-hr.md) | — |
| dot-dopemine | Engagement intelligence engine | publishing | 1.0.0 | 0.58 | Dopamine | [doc](platforms/dot-dopemine.md) | — |
| dot-emall | Marketplace platform | publishing | 1.0.0 | 0.58 | Marketplace | [doc](platforms/dot-emall.md) | — |
| dot-ehail | E-hailing entrepreneurship platform | publishing | 1.0.0 | 0.58 | Logistics | [doc](platforms/dot-ehail.md) | — |
| dot-agents | AI agent orchestration platform | publishing | 1.0.0 | 0.58 | Colony | [doc](platforms/dot-agents.md) | — |
| dot-auction | Auction marketplace | publishing | 1.0.0 | 0.58 | Marketplace | [doc](platforms/dot-auction.md) | — |
| dot-central | Operational Intelligence Center | publishing | 1.0.0 | 0.58 | Mining | [doc](platforms/dot-central.md) | — (dispatch workflow node IDs defined in doc §2) |
| dot-projects | Project management | publishing | 1.0.0 | 0.58 | Delivery | [doc](platforms/dot-projects.md) | — |
| dot-tasks | Task management | publishing | 1.0.0 | 0.58 | Delivery | [doc](platforms/dot-tasks.md) | — |
| dot-design | Enterprise design system | publishing | 1.0.0 | 0.58 | UX | [doc](platforms/dot-design.md) | — |
| dot-finance | Financial platform | publishing | 1.0.0 | 0.58 | Finance | [doc](platforms/dot-finance.md) | — |

\* interim assignment pending open question below.

Per-platform `platforms/<platform>.md` documents are produced one per session by prompt 05 ("Using 05, integrate Dot.<Platform>").

## 3. Universal Onboarding Procedure (invariant)

```mermaid
flowchart TD
    A[1. Author & sign platform.dkp.json] --> B[2. Register: automated manifest validation]
    B -->|DKP_SCHEMA_INVALID / DKP_SIG_INVALID| A
    B --> C[3. Drop platforms/<new>.md from template]
    C --> D[4. Publish 'hello' Knowledge Pack:<br/>entity_model + event_model]
    D --> E[5. Dot.Brain auto-creates graph namespaces<br/>+ baseline relationships]
    E --> F[6. Trust starts probationary at 0.50<br/>grows with validated packs]
    F --> G[Status: registered → publishing → full-loop]
```
*Six steps, never more, never fewer — identical for a mining ERP, a trading platform, or a domain that does not exist yet.*

| Step | Actor | Automated check | Failure handling |
|---|---|---|---|
| 1. Manifest | Platform team | Schema + signature round-trip test | Errors returned with hints |
| 2. Register | Registry Agent | Manifest validation, tenant topic provisioning, key registration | T3 human activation approval |
| 3. Knowledge doc | Platform team + domain agent | Template contract check (Documentation Agent) | PR review cycle |
| 4. Hello pack | Platform team | Full DKP validation pipeline (§3.1) | Validation report returned |
| 5. Namespaces | Dot.Brain (automatic) | Graph namespace + baseline `relates-to` edges created | Auto-retry; Registry Agent alerted |
| 6. Trust | Automatic | Starts 0.50; probationary rules per DKP §3.2 | — |

**Deactivation** is the reverse path, gated T4: knowledge-retention plan required; the platform's knowledge is never deleted — its nodes are marked deprecated with provenance and its tenant topics closed.

## 4. Worked Example: Onboarding a Platform That Doesn't Exist Yet

Suppose **Dot.Logistics** (fleet routing SaaS) launches in 2029:

1. Team authors `platform.dkp.json`: `platform: dot-logistics`, Ed25519 key, publishes `entity_model`, `event_model`, `metric`, `insight`; subscribes to `reliability` and `workflow` advisories.
2. Registry Agent validates; Chief Knowledge Engineer approves activation (T3); tenant topics `dkp/dot-logistics/{publish,response}` provisioned.
3. Team drops `platforms/dot-logistics.md` from the template; adds one row to §2 above.
4. Hello pack publishes entities (Vehicle, Route, Depot) and events (`route.completed`, `vehicle.delayed`).
5. Graph namespace `dot:node:logistics:*` auto-created; baseline edges inferred: `relates-to` Dot.Mines haulage and Dot.Farms harvest-logistics patterns (shared vehicle-routing ontology) — flagged at confidence 0.60 for Knowledge Agent review.
6. Trust 0.50, probationary. After ~20 validated packs and 2 accepted PRs, trust reaches 0.65; status `full-loop`. **Zero Dot.Brain files changed except `platforms/dot-logistics.md` and one registry row.**

## 5. Metrics of Success

| Metric | Target |
|---|---|
| `registry.onboarding_invariant_violations` | 0, always |
| `registry.median_onboarding_time` (manifest → first ingested pack) | ≤ 5 days |
| `registry.stale_entries` at monthly audit | 0 |
| `registry.platforms_at_full_loop` | 100% within 2 quarters of registration |
| `registry.manifest_validation_turnaround` | ≤ 24 h |

## 6. Open Questions

| Question | Owner |
|---|---|
| Domain-agent home for Dot.Notify (infrastructure platform, not commerce) — dedicated Platform-Infra agent or Architecture Agent? | Governance Agent → Chief AI Engineer |
| Dot.HR knowledge involves employee PII — does it need a stricter aggregation floor than Finance's n ≥ 20? | Security Agent → Security Officer |
| ~~Should `dot-brain` itself publish packs about its own operation (self-knowledge), and under which trust rules?~~ **Resolved 2026-08-01:** yes, under stricter-than-tenant rules — human verification only (no self-grading), unsuppressable drift channel; see platforms/dot-brain.md §4/§7 | Governance Agent → Chief Intelligence Architect |

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Platform Integrator (prompt 05, AI) | Initial registry (21 platforms) + universal onboarding procedure |
| 1.0.1 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-farms and dot-mines integration packages published: status → publishing, trust baselines advanced (0.58/0.62), open gaps updated |
| 1.0.2 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-central integration package published: status → publishing (0.58); dispatch-workflow node ID gap closed |
| 1.0.3 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-emall integration package published: status → publishing (0.58) |
| 1.0.4 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-billing integration package published: status → publishing (0.58); aggregation-floor config gap closed |
| 1.0.5 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-analytics integration package published: status → publishing (0.58); KPI-catalog sync gap closed |
| 1.0.6 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-dopemine integration package published: status → publishing (0.58); prohibited-metric list gap closed |
| 1.0.7 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-pulse integration package published: status → publishing (0.58); discussion-pack privacy review gap closed |
| 1.0.8 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-notify integration package published: status → publishing (0.58); domain-agent assignment resolved (Marketplace* → Documentation) |
| 1.0.9 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-hr integration package published: status → publishing (0.58); PII classification review gap closed; agent Business* → People |
| 1.0.10 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-ehail integration package published: status → publishing (0.58); fleet entity model gap closed; agent Marketplace → Logistics |
| 1.0.11 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-auction integration package published: status → publishing (0.58); auction-mechanism scoping gap closed |
| 1.0.12 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-projects and dot-tasks integration packages published (paired session — shared boundary contract): status → publishing (0.58); agent Business → Delivery |
| 1.0.13 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-charts integration package published: status → publishing (0.58); compliance gate wiring gap closed (bidirectional, MNPI screen) |
| 1.0.14 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-finance integration package published: status → publishing (0.58); regulatory watch setup gap closed; three queued cross-platform questions answered |
| 1.0.15 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-plug integration package published: status → publishing (0.58); extension entity model gap closed; agent Marketplace → Extension |
| 1.0.16 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-memory integration package published: status → publishing (0.58); retrieval SLA contract gap closed; brain.memory.md straggler metrics homed |
| 1.0.17 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-agents integration package published: status → publishing (0.58); colony runtime contract gap closed; five domain-agent assignments recorded pending brain.agents.md promotion |
| 1.0.18 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-design integration package published: status → publishing (0.58); token-consumption contract gap closed; four inherited open questions settled |
| 1.0.19 | 2026-08-01 | Platform Integrator (prompt 05, AI) | dot-brain self-referential doc published: final gap closed, self-knowledge OQ resolved. F-06 complete — 21 of 21 platforms documented |
