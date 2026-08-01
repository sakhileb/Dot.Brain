---
title: os/ — Dot Ecosystem Operating System
version: 1.0.0
status: active
owners: [Sakhile Bhayi]
last-review: 2026-08-01
---

# os/ — Dot Ecosystem Operating System

This directory is the ecosystem-wide **operating doctrine** for the entire ~20-platform Dot Ecosystem — the layer that sits *above* the ~35 `brain.*.md` documents one level up in this repository. `brain.*.md` is Dot.Brain's own detailed technical specification: its knowledge graph, its reasoning engine, its governance, its own resilience and security posture, as a platform in its own right. `os/` is one level up from that: how the whole ecosystem of ~20 platforms — including Dot.Brain itself — is actually run, built, secured, and owned. Where the two overlap (resilience, security, governance), the `os/` document states the ecosystem-wide doctrine and cross-links down to the `brain.*.md` document that covers Dot.Brain's own internal mechanics in depth; neither document re-derives the other.

Every document in this set follows [MANIFESTO.md](../MANIFESTO.md): no placeholder content, every claim explainable, every recommendation measurable, human judgment never skipped. Every document here was written grounded in this session's real, verifiable state — 15 platforms built out with real Laravel/Jetstream code and audited for security, 5 still empty, real bugs found and fixed, nothing test-executed (no local PHP/Postgres/Docker in the environment that wrote it). Where a document extrapolates beyond verified fact, it says so in its own text or Open Questions.

---

## The 20 documents, plus Appendix

| # | Document | One-line description |
|---|---|---|
| 01 | [Executive Vision](01-Executive-Vision.md) | What the Dot Ecosystem is, why it exists, and what structurally separates it from a normal multi-product SaaS portfolio. |
| 02 | [Engineering Loop](02-Engineering-Loop.md) | The real, environment-honest procedure for advancing any platform's codebase — bounded passes, human-reviewed before every push. Supersedes the earlier "fully autonomous, no check-ins" instruction, which was tried and replaced. |
| 03 | [Business Automation](03-Business-Automation.md) | What automation honestly means for a solo operator running ~20 AI-built platforms — what's worth automating ecosystem-wide, and what can't be automated yet because no platform publishes data to another. |
| 04 | [Dot.Brain](04-Dot-Brain.md) | An OS-altitude summary of what Dot.Brain is and where it actually stands — the ownership boundary, the publish→ingest→reason→PR loop, and the gap between the protocol as specified and as adopted. |
| 05 | [Knowledge Protocol](05-Knowledge-Protocol.md) | Plain-language explanation of the DKP (Dot Knowledge Pack) protocol, the six-step onboarding path, and a realistic first move for a real platform to start publishing. |
| 06 | [Design System](06-Design-System.md) | The visual and UX conventions actually built and repeated across 15 platforms — logo system, dark/light toggle, the notification-bell pattern, empty/loading states. |
| 07 | [Development Standards](07-Development-Standards.md) | Coding standards actually observed while executing the Engineering Loop — authorization, no-placeholder-output, testing, and commit conventions. |
| 08 | [Agent System](08-Agent-System.md) | How AI agents actually operate as the engineering workforce: scoped prompts, explicit boundaries, commit-never-push, structured reports, the scale-warning pattern for complex codebases. |
| 09 | [Business Operating System](09-Business-Operating-System.md) | How the ecosystem runs day to day — cadences, decision tiers, and how decisions get made with one owner and an AI execution layer. |
| 10 | [Owner Control](10-Owner-Control.md) | The approval matrix, the autonomous-agent boundary, credential/secrets ownership, and the flag-vs-fix escalation path — the standing policy this session already used in practice. |
| 11 | [AI Decision Engine](11-AI-Decision-Engine.md) | Honest description of how AI actually drives decisions today (scoped, human-reviewed engineering work) versus the automated reasoning/recommendation engine Dot.Brain will become once real DKP data exists. |
| 12 | [README Automation](12-README-Automation.md) | A practical convention for keeping every platform's README accurate, given multiple platforms were found with fictional/aspirational README content this session. |
| 13 | [Engineering State](13-Engineering-State.md) | The live, current status of every platform against the Engineering Loop — the ground-truth tracker other documents reference rather than restate. |
| 14 | [Credit Optimization](14-Credit-Optimization.md) | AI/compute cost discipline — bounded-scope agents, one platform per pass, human review before expensive actions. ("Credit" = compute spend, not Dot.Finance's retracted lending scope.) |
| 15 | [MEGA v2](15-MEGA-v2.md) | The composite scoring model — how platform maturity, security posture, and ecosystem-integration readiness combine into one comparable score per platform. |
| 16 | [Disaster Recovery](16-Disaster-Recovery.md) | What DR actually means today: code recovery is solved via git; production data recovery is an open, unresolved gap. Extends ADR-0007's RTO/RPO tier model to platform scope. |
| 17 | [Security](17-Security.md) | Ecosystem-wide security doctrine grounded in this session's real findings — the recurring vulnerability classes as a standing audit checklist, and an honest statement that nothing has been penetration-tested. |
| 18 | [Platform Lifecycle](18-Platform-Lifecycle.md) | The five stages a platform actually moves through — empty scaffold → hand-authored app → engineering-loop pass → DKP publishing (not yet reached) → full integration (not yet reached) — plus the domain-mismatch failure mode found in Dot.Central, Dot.Design, and Dot.Finance. |
| 19 | [Knowledge Packs](19-Knowledge-Packs.md) | Hands-on companion to doc 05: one worked example per DKP payload type, including a full incident-report pack for the real Dot.Emall checkout race-condition bug, plus exactly what blocks each of the 15 real platforms from publishing today. |
| 20 | [Continuous Evolution](20-Continuous-Evolution.md) | Why "done" never arrives, and how the platform-loop (audit → fix → test → document → commit) is designed to run again indefinitely. |
| App. | [Appendix](Appendix.md) | Full platform table with real repo URLs and build status, the logo-source mapping, and a glossary excerpt. |

## Reading order, by audience

**The owner, catching up after time away.** Start with [01-Executive-Vision.md](01-Executive-Vision.md) for the one-page truth of where things stand, then [13-Engineering-State.md](13-Engineering-State.md) for what's actually live right now, then [09-Business-Operating-System.md](09-Business-Operating-System.md) §3 for what cadence you're re-entering. If a security or recovery question is the reason you're catching up, go straight to [17-Security.md](17-Security.md) or [16-Disaster-Recovery.md](16-Disaster-Recovery.md) instead — both are written to be read standalone.

**A new AI agent, starting a session.** Read [../CLAUDE.md](../CLAUDE.md) first (it governs this repository), then [../MANIFESTO.md](../MANIFESTO.md), then [01-Executive-Vision.md](01-Executive-Vision.md) for ecosystem context, then [02-Engineering-Loop.md](02-Engineering-Loop.md) and [07-Development-Standards.md](07-Development-Standards.md) before touching any platform code, then [08-Agent-System.md](08-Agent-System.md) for how your own work should be scoped and reported. Check [13-Engineering-State.md](13-Engineering-State.md) for the specific platform you've been assigned before assuming its current state.

**An external contributor, if that ever happens.** There is no external-contribution path today — [10-Owner-Control.md](10-Owner-Control.md) explains why that's a deliberate current state (one accountable human, sole GitHub credential holder), not an oversight. [01-Executive-Vision.md](01-Executive-Vision.md) and [../MANIFESTO.md](../MANIFESTO.md) are the right starting point for understanding what the ecosystem is actually trying to be before asking how to join it.

## How this set relates to `brain.*.md`

`os/` is ecosystem-wide operating doctrine: how ~20 platforms (Dot.Brain among them) are built, secured, recovered, and owned as a whole. `brain.*.md` is the detailed technical specification layer, one level down, for Dot.Brain specifically — its knowledge graph, reasoning engine, governance model, agent colony, and its own internal resilience and security framework. Read `os/` to understand the ecosystem; read `brain.*.md` to understand how Dot.Brain itself works. Neither replaces the other, and where they cover adjacent ground (security, resilience, governance), the `os/` document says so explicitly and cross-links down rather than re-deriving it.
