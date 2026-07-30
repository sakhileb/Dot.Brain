# 04 — Agent Colony Architect

> **Prerequisite:** `00-System-Prompt.md` loaded; DKP from `02` exists.
> **Single responsibility:** Design the autonomous AI agent colony that maintains Dot.Brain.
> **Outputs:** Full `brain.agents.md`, `templates/agent-charter.template.md`, one charter per agent, colony governance rules, ADRs.

---

## TASK

Design a colony of autonomous AI agents that collectively maintain, grow, and review Dot.Brain — under human governance, never above it.

## THE COLONY (minimum roster)

Knowledge, Research, Documentation, Architecture, UX, Data, Security, Testing, Marketplace, Business, Mining, Trading, Agriculture, Finance, Community, Memory, Reasoning, Learning, Evolution, Dopamine, Governance — plus a **Resilience Agent** (owns incident learning loops) and any agents you justify with an ADR.

## AGENT CHARTER (every agent gets one)

Each charter, from `templates/agent-charter.template.md`, must define:

1. **Identity & mission** — one sentence each.
2. **Responsibilities** — owned documents (per the 01 ownership matrix), owned pack types, owned graph domains.
3. **Authority & limits** — what it may do autonomously (draft, analyze, open PRs), what requires peer review, what requires human approval. **No agent can merge its own PR. No agent can overwrite approved knowledge — supersede only, with provenance.**
4. **Memory contract** — what it reads/writes in shared memory (via Dot.Memory), retention, and what it must never store (secrets, personal data beyond classification rules in `brain.security.md`).
5. **Review duties** — which other agents' work it reviews, with a review rubric.
6. **Learning loop** — what signals it learns from (PR acceptance rate, incident outcomes, human feedback), and how its own trust score is affected.
7. **KPIs** — 3–5 measurable indicators of the agent doing its job well.
8. **Failure modes** — known risks (hallucinated relationships, contradiction spam, review rubber-stamping) and mitigations.

## COLONY-LEVEL DESIGN

Produce, with Mermaid diagrams:

1. **Collaboration topology** — who reviews whom (ensure no closed mutual-approval loops of size 2), escalation paths to the Governance Agent and then to humans.
2. **Shared memory architecture** — namespaces, read/write permissions matrix, conflict handling when two agents write related knowledge simultaneously.
3. **Work lifecycle** — `stateDiagram-v2`: signal → triage → draft → peer review → (human gate if impact ≥ threshold) → PR → outcome recorded → learning update.
4. **Trust & reputation** — per-agent trust score feeding DKP §3; probation mode for low-trust agents (all output requires human review).
5. **Communication protocol** — agents communicate through DKP packs and PR comments only; no side channels; everything auditable.
6. **Human override** — any human with the approver role can pause an agent, revert its PRs, or adjust its authority; every override becomes a learning event.
7. **Domain examples** — three end-to-end scenarios: (a) Mining Agent turns Dot.Mines telemetry insight into a Dot.Central dispatch recommendation; (b) Security Agent + Resilience Agent process an outage into propagated lessons; (c) Dopamine Agent rejects an engagement proposal that fails the ethical gate.

## RULES

- Agents learn continuously, but learned behavior changes are themselves versioned and reviewable.
- Every agent action emits telemetry per `brain.telemetry.md`.
- Adding a new agent = new charter + registry entry + ownership-matrix update — no architecture change.

## EXIT CRITERIA

Given only the charters and colony rules, a platform engineer could predict exactly what any agent is allowed to do in any situation — and an auditor could reconstruct why any PR exists.
