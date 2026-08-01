---
title: Dot Ecosystem — MEGA v2 (Composite Platform Scoring Model)
version: 1.0.0
status: active
owners: [Sakhile Bhayi]
last-review: 2026-08-01
---

# 15 — MEGA v2

Purpose: a single, comparable score per platform — synthesizing engineering completeness, security posture, design/UX consistency, documentation accuracy, and ecosystem-integration readiness — so the owner can answer "which platform needs attention next" without re-reading five separate documents. This is v2 because v1 would have scored against the *aspirational* per-platform vision in `platforms/*.md`; v2 scores against *what's actually in each repository*, per [18-Platform-Lifecycle.md](18-Platform-Lifecycle.md)'s honesty rule. A high score here means "solid relative to the other Dot platforms as they exist today," never "production-ready" or "secure" in any absolute sense — see [17-Security.md](17-Security.md) §4 for why no platform can honestly claim either yet.

> **Related documents:** [os/13-Engineering-State.md](13-Engineering-State.md) — the raw per-platform status this model scores · [os/17-Security.md](17-Security.md) — the findings behind the security dimension · [os/18-Platform-Lifecycle.md](18-Platform-Lifecycle.md) — the stage model the lifecycle dimension is built on · [os/06-Design-System.md](06-Design-System.md) — the conventions the design dimension checks against · [os/19-Knowledge-Packs.md](19-Knowledge-Packs.md) — the blockers behind the integration dimension.

---

## 1. Why a scoring model, and why it's dangerous if misused

A single number is useful for triage and dangerous for false confidence. This model exists to answer one question well — *relative to the other ~20 Dot platforms, where should the next engineering pass go?* — and no other question. It must never be read as "platform X scored 8/10, therefore it's safe to take real payments on it" (see [17-Security.md](17-Security.md) §4: nothing has been penetration-tested, so no score here implies production safety). Any score is only as current as [13-Engineering-State.md](13-Engineering-State.md); re-derive before relying on a number older than a few weeks.

## 2. The five dimensions

| Dimension | What it measures | Source of truth |
|---|---|---|
| **Engineering completeness (E)** | Does the platform have real, non-placeholder domain code matching its stated purpose? | Each platform's own `wiki.md`, cross-checked against `platforms/<id>.md` |
| **Security posture (S)** | Has the platform been through at least one bounded security/tech-debt scan, and were findings actually fixed (not just noted)? | [os/17-Security.md](17-Security.md) §2, [os/13-Engineering-State.md](13-Engineering-State.md) §2 |
| **Design/UX consistency (D)** | Does the platform match the ecosystem's real conventions — real logo, dark/light toggle, notification pattern where applicable, empty/loading states? | [os/06-Design-System.md](06-Design-System.md) |
| **Documentation accuracy (Doc)** | Does the README/wiki describe what's actually built, not an aspirational or fictional feature set? | [os/12-README-Automation.md](12-README-Automation.md), each platform's `wiki.md` |
| **Ecosystem-integration readiness (I)** | How close is the platform to actually publishing a DKP, per the blocker list? | [os/19-Knowledge-Packs.md](19-Knowledge-Packs.md) §4 |

Each dimension is scored 0–2 per platform:

| Score | Meaning |
|---|---|
| 0 | Not started / not present |
| 1 | Partially present, with a known, named gap |
| 2 | Present and matches the ecosystem's established real convention for that dimension |

**MEGA score = E + S + D + Doc + I, out of 10.** No dimension is weighted above another — a platform that is beautifully designed but insecure should not outscore one that is secure but plain; the point of an unweighted sum is to make every gap visible in the total rather than hidden by a strong dimension elsewhere.

## 3. Scored: the 15 platforms with real code

Derived directly from [os/13-Engineering-State.md](13-Engineering-State.md) §2 and [os/19-Knowledge-Packs.md](19-Knowledge-Packs.md) §4 at time of writing. Every platform scores **I = 0** — none has a DKP manifest, signing key, or publish job yet, per doc 19's blocker table; this is the ecosystem's single most consistent gap and the reason no platform can score above 8/10 today.

| Platform | E | S | D | Doc | I | MEGA | Why |
|---|---|---|---|---|---|---|---|
| Dot.Tasks | 2 | 2 | 2 | 2 | 0 | 8 | Real cross-tenant vuln found *and* fixed; full loop pass complete. |
| Dot.Emall | 2 | 2 | 2 | 2 | 0 | 8 | Real checkout race condition found *and* fixed; full loop pass complete. |
| Dot.Auction | 2 | 2 | 2 | 2 | 0 | 8 | Reserve-price leak found *and* fixed; full loop pass complete. |
| Dot.Finance | 2 | 2 | 2 | 2 | 0 | 8 | IDOR-hardened from a near-empty starting point; full loop pass complete. |
| Dot.Mines | 2 | 2 | 2 | 2 | 0 | 8 | Tenant-scoping gap found *and* fixed; full loop pass complete. |
| Dot.Charts | 2 | 2 | 2 | 2 | 0 | 8 | Fake-output finding fixed via honest demo labeling; full loop pass complete. |
| Dot.Billing | 2 | 2 | 2 | 2 | 0 | 8 | Missing invoice-access Policy found and fixed. |
| Dot.Ehail | 2 | 2 | 2 | 2 | 0 | 8 | Missing driver-profile Policy found and fixed. |
| Dot.Projects | 2 | 2 | 2 | 2 | 0 | 8 | Task-assignment authorization gap found and fixed. |
| Dot.Agents | 2 | 1 | 2 | 2 | 0 | 7 | UI gap (dead notification bell) fixed, not a security gap — scored S=1 because the pass deliberately did not audit the governance/scoring internals (out of bounded scope, per [08-Agent-System.md](08-Agent-System.md)'s scale-warning pattern), so "no finding" here means "not fully checked," not "checked and clean." |
| Dot.Pulse | 2 | 1 | 2 | 2 | 0 | 7 | Same scale-warning caveat as Dot.Agents — the AI moderation/knowledge-graph internals were explicitly out of scope. |
| Dot.Analytics | 2 | 1 | 2 | 2 | 0 | 7 | Same caveat — the 17-engine intelligence service and knowledge graph were out of scope. |
| Dot.Notify | 1 | 1 | 2 | 2 | 0 | 6 | E=1: the platform's core purpose (inbound webhooks) has no working endpoint at all — a real completeness gap, not just a security one. |
| Dot.Central | 2 | 1 | 2 | 2 | 0 | 7 | UI-parity pass only this round, no fresh full security scan (see [13-Engineering-State.md](13-Engineering-State.md) open questions). |
| Dot.Design | 2 | 1 | 2 | 2 | 0 | 7 | Same as Dot.Central — UI-parity pass, no fresh full security scan. |

**Ecosystem mean (15 platforms): 7.3 / 10.** The ceiling every platform is bumping against is the same one dimension: `I = 0` across the board.

## 4. The 5 empty scaffolds

Score **0 / 10** by definition — no code exists yet. Listed for completeness, not to imply deficiency: an empty scaffold correctly scores zero on every dimension because there is nothing to measure. See [18-Platform-Lifecycle.md](18-Platform-Lifecycle.md) stage 1.

| Platform | MEGA |
|---|---|
| Dot.Plug | 0 |
| Dot.Farms | 0 |
| Dot.HR | 0 |
| Dot.Dopemine | 0 |
| Dot.Memory | 0 |

## 5. What this model says to do next

Two distinct signals, not one priority queue:

1. **Lowest-scoring platform with real code (Dot.Notify, 6/10):** its `E=1` is a completeness gap in its actual core purpose, not a security afterthought — the next pass should build the missing webhook endpoint with real signature verification, not just re-run the standard loop checklist.
2. **`Dot.Agents`, `Dot.Pulse`, `Dot.Analytics`, `Dot.Central`, `Dot.Design` (all S=1):** these five never got a full, unscoped security scan — their internals were deliberately fenced off per [08-Agent-System.md](08-Agent-System.md)'s scale-warning pattern. That was the right call for a single bounded pass, but it means their S=1 is a statement about *what wasn't checked*, not a clean bill of health. A dedicated, deeper pass on exactly these five internals (governance stack, moderation pipeline, intelligence engines, AI-agent domain, design internals) is the highest-value next security action, separate from the platform-loop's general checklist.
3. **Every platform's `I=0`:** this is the whole ecosystem's real bottleneck, not any single platform's fault. Closing it for even one platform (per [os/19-Knowledge-Packs.md](19-Knowledge-Packs.md) §4's blocker list — Dot.Billing is the best-scoped candidate) would be the first real end-to-end DKP round-trip this ecosystem has ever had, and would let this scoring model's `I` dimension mean something beyond zero for the first time.

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Sakhile Bhayi | Initial scoring model — 5 dimensions, unweighted 0–10 composite, scored against the real state of all 15 built platforms plus the 5 empty scaffolds. |

## Open Questions

- Should `I` (integration readiness) be weighted less than the other four dimensions until at least one platform closes it, so the ecosystem mean isn't perpetually capped by a gap every platform shares equally? Recommend against it for now — an unweighted score keeps the gap visible rather than normalizing it away.
- Should the five `S=1` platforms (Agents, Pulse, Analytics, Central, Design) be re-scored `S=0` instead, to more sharply distinguish "not fully checked" from "checked, one thing found and fixed"? Current model treats them as better than an unaudited platform but explicitly caveats why — worth revisiting once each gets its dedicated deeper pass.
- This model has never been re-run — it reflects one point-in-time snapshot from [13-Engineering-State.md](13-Engineering-State.md). It should be re-derived every time that document changes, not left stale.
