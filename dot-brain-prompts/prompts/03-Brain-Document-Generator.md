# 03 — Brain Document Generator

> **Prerequisite:** `00-System-Prompt.md` loaded; structure from `01` and DKP from `02` exist.
> **Single responsibility:** Generate one `brain.*.md` document (or one `platforms/*.md` document) per session, completely and production-ready.
> **Invocation:** "Using 03, generate `brain.<domain>.md`."

---

## TASK

Write the requested document to its final, production-ready state. One document per session. No placeholders, no "TBD", no thin sections.

## UNIVERSAL DOCUMENT CONTRACT

Every document must contain, in order:

1. **Front-matter** — title, version (`1.0.0` on first completion), status, owning agent, reviewing agent, human approver role, last-review date.
2. **Purpose** — one paragraph: what this document governs and who reads it.
3. **Related documents** — cross-links (minimum 3) with one line each on the relationship.
4. **Body** — per the domain briefs below. Include at least one Mermaid diagram, at least one table, and at least one concrete worked example drawn from a real ecosystem scenario (mining, agriculture, trading, e-hailing, marketplace, HR).
5. **Metrics of success** — how we will know this domain is working (specific, measurable).
6. **Open questions** — honest unknowns, each assigned to an agent or human role.
7. **Change log** — table: version, date, author (human/AI), summary.

## DOMAIN BRIEFS

- **brain.identity.md** — What Dot.Brain is/is not; the ownership boundary; the Manifesto applied; relationship to Dot.Memory (Brain reasons, Memory stores) and Dot.Agents (Brain governs knowledge, Agents execute).
- **brain.architecture.md** — Layered architecture (ingestion → validation → graph → reasoning → recommendation → delivery); multi-tenant isolation; scalability model; technology-agnostic component contracts; C4-style Mermaid diagrams.
- **brain.vision.md** — 20-year vision, staged capability roadmap, what "smarter every day" means measurably.
- **brain.reasoning.md** — Reasoning engine: root cause analysis, decision trees, counterfactual thinking, risk analysis, scenario planning; business/market/operational recommendation pipelines; explainability format (every conclusion ships with evidence chain + confidence); confidence scoring math.
- **brain.learning.md** — Continuous learning loops: what is learned from packs, PR outcomes, adoption data, incidents; feedback ingestion; guardrails against learning from unverified or low-trust sources.
- **brain.memory.md** — Memory orchestration with Dot.Memory: hot/warm/cold knowledge tiers, retrieval contracts, forgetting policy (supersede, never delete; deprecate with provenance).
- **brain.relationships.md** — The knowledge graph: node types (Users, Businesses, Projects, Tasks, Machines, Employees, Markets, Products, Communities, AI Agents, Dashboards, Reports, Events, Insights, Documents, Discussions, Workflows, Recommendations), edge taxonomy, automatic relationship inference rules (with confidence), graph governance, `erDiagram`.
- **brain.agents.md** — Summary of the Agent Colony (canonical detail lives in output of prompt 04); registry of agents, shared-memory contract, PR etiquette.
- **brain.analytics.md** — How Brain consumes/produces analytics with Dot.Analytics; ecosystem-level KPIs.
- **brain.design.md** — Documentation & UX standards; how Dot.Design tokens/components shape Brain-generated dashboards and reports.
- **brain.security.md** — Threat model, tenant isolation, signing, secrets, least privilege for agents, data classification, POPIA/GDPR posture.
- **brain.api.md** — External API surface: DKP endpoints, query API, recommendation API, webhooks; auth, versioning, rate limits, error contract.
- **brain.search.md / brain.semantic.md** — Lexical + semantic retrieval, embedding strategy, hybrid ranking, semantic layer ontology.
- **brain.telemetry.md** — What Brain measures about itself; health, ingestion lag, reasoning latency, recommendation acceptance rate.
- **brain.workflows.md** — Cross-platform workflow models and how Brain recommends workflow improvements.
- **brain.metrics.md** — Canonical metric definitions ecosystem-wide (single source of truth for metric names, formulas, units).
- **brain.recommendations.md** — Recommendation object lifecycle, impact declarations (business/user/dopamine), acceptance tracking, ROI measurement.
- **brain.evolution.md** — The Evolution Engine: reads every Knowledge Pack; identifies patterns; detects duplicate ideas and contradictions; measures feature adoption, business value, user happiness, productivity gains, ROI; suggests improvements; generates experiments and A/B testing proposals; produces architectural recommendations; tracks long-term trends.
- **brain.dopemine.md** — The scientific engagement engine. Hard rule: never optimize for addiction or screen time. Optimize for learning, achievement, business growth, mastery, productivity, community, purpose, confidence, momentum, habit formation, progress. Include the ethical review gate every engagement recommendation must pass.
- **brain.community.md** — Dot.Pulse integration: how discussions become knowledge, community feedback loops, reputation vs trust score.
- **brain.business.md** — Business automation intelligence: opportunity detection, cross-platform value chains (e.g., Dot.Farms produce → Dot.Emall listing → Dot.Billing → Dot.Analytics).
- **brain.governance.md** — Audit, versioning, explainability, traceability, reviewability; decision rights; escalation paths (canonical detail in output of prompt 06).
- **brain.resilience.md** — Resilience & Continuity Framework (canonical detail in output of prompt 06); summary and cross-links.
- **brain.experiments.md / brain.failures.md / brain.success.md** — Experiment registry format; failure knowledge base (anti-fragility ledger); success pattern library. Failures and successes are symmetric first-class knowledge.
- **brain.events.md** — Ecosystem event taxonomy and event-to-knowledge pipeline.
- **brain.patterns.md** — Cross-platform pattern catalog (detected by Evolution Engine), pattern promotion rules.
- **brain.personas.md** — Reader/user personas (miner, farmer, trader, entrepreneur, HR manager, developer, executive, AI agent) and how Brain adapts explanations per persona.
- **brain.operating_model.md** — Cadences: ingestion (continuous), review (weekly), evolution report (monthly), governance audit (quarterly), resilience drill (quarterly).
- **brain.future.md** — Extension points, integration path for unknown future platforms, explicitly reserved namespaces.
- **platforms/<platform>.md** — Use `templates/platform-knowledge.template.md`: platform purpose, entities owned, events emitted, packs published, intelligence consumed, active recommendations, incident history summary, integration status.

## EXIT CRITERIA (per document)

The document could be merged to `main` today, passes the contract above, and the 07-Reviewer would score it ≥ 4/5 on completeness, consistency, and cross-linking.
