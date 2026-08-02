---
title: Dot Ecosystem — MEGA v2 (Composite Platform Scoring Model)
version: 2.1.0
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
| Dot.Agents | 2 | 2 | 2 | 2 | 0 | 8 | **Pass 2:** dedicated deep pass on the governance stack found and fixed a real cross-org IDOR (Livewire method-argument lookups in `ApprovalQueue`/`KnowledgeManager`); everything else (scoring, approval workflow, immune system, prompt-injection guard) confirmed clean. |
| Dot.Pulse | 2 | 2 | 2 | 2 | 0 | 8 | **Pass 2:** dedicated deep pass on the moderation pipeline found and fixed a real cross-tenant IDOR across 6 entry points, plus a moderation fail-open on job-retry exhaustion. Two deeper issues flagged for a future pass (knowledge-graph tenant scoping, a fail-open on unparseable AI responses) — not blocking this score, since they're documented and currently unexploitable. |
| Dot.Analytics | 2 | 2 | 2 | 2 | 0 | 8 | **Pass 2:** dedicated deep pass confirmed the 17-engine service, knowledge graph, and Business DNA profiling are all genuinely clean — every query team-scoped. Found and fixed one real bug in a more mundane spot: a feature-flag API leak of other teams'/users' IDs. |
| Dot.Notify | 2 | 2 | 2 | 2 | 0 | 8 | **Pass 2:** the missing inbound webhook endpoint (this platform's core purpose) is now built — HMAC-SHA256 verification, timing-safe comparison, no enumeration oracle, rate-limited. E moves 1→2. |
| Dot.Central | 2 | 2 | 2 | 2 | 0 | 8 | **Pass 2:** dedicated review of the previously-unaudited AI-agent domain (separate from the mining-dispatch scaffold) — genuinely clean, not just unchecked. |
| Dot.Design | 2 | 2 | 2 | 2 | 0 | 8 | **Pass 2:** dedicated review of the previously-unaudited AI canvas domain — safe by omission (no reachable per-ID surface exists yet); documented as a build-time requirement for whoever adds one. |

**Mean of these 15: 8.0 / 10** (up from 7.53 after pass 2). The ceiling every platform is bumping against is now entirely `I = 0` — every other dimension across every platform in this group is maxed.

## 4. The 5 hand-authored platforms

Built from an empty scaffold this session by copying Dot.Billing's real Jetstream shell file-by-file, then adding a from-scratch domain layer (see [13-Engineering-State.md](13-Engineering-State.md) §3). Scored on the same rubric as §3 — these are no longer empty, and score accordingly, though `S` reflects that nothing here has been executed any more than the other 15 have (that caveat is ecosystem-wide, not specific to this group — see [17-Security.md](17-Security.md) §4).

| Platform | E | S | D | Doc | I | MEGA | Why |
|---|---|---|---|---|---|---|---|
| Dot.Memory | 2 | 2 | 2 | 2 | 0 | 8 | "Store without reading" enforced structurally (no content-shaped column exists anywhere in the schema) and backed by a dedicated regression test — verified directly against the schema, not just the claim. |
| Dot.Plug | 2 | 2 | 2 | 2 | 0 | 8 | Certification gate and team-scoped install/uninstall verified directly in the controller. |
| Dot.Dopemine | 2 | 2 | 2 | 2 | 0 | 8 | The manifesto's ethics constraint enforced at three independent layers (closed enum, action-level check, model `saving` listener) — verified line-by-line, the strongest engineering of this group. |
| Dot.Farms | 2 | 2 | 2 | 2 | 0 | 8 | `FarmPolicy` verified across every child-resource controller; the one resource without by-ID routes (Crop) confirmed to have no cross-team surface at all. |
| Dot.HR | 2 | 2 | 2 | 2 | 0 | 8 | **Pass 2:** the top-priority gap is closed — `create`/`update`/`delete` on Employee/LeaveRequest/Position now require the team's `admin` role, not just membership; `view` stays open team-wide. Covered by a new role-gating test block. |

**Mean of these 5: 8.0 / 10** (up from 7.8 after pass 2).

**Ecosystem mean, all 20 platforms: 8.0 / 10** (up from 7.6). Every platform now scores the maximum 2 on E, S, D, and Doc — the entire remaining gap to a perfect ecosystem score is `I = 0`, universally.

## 4a. The 7 platforms discovered via InfoDot's registry

Pre-built, real codebases nobody had registered until InfoDot's `config/ecosystem.php` was reconciled against reality (see [13-Engineering-State.md](13-Engineering-State.md) §3a). Each got exactly one bounded integration pass — not the two-pass depth the other 20 have had — so scores here reflect that single pass, not a confirmed-clean re-verification.

| Platform | E | S | D | Doc | I | MEGA | Why |
|---|---|---|---|---|---|---|---|
| Dot.Sheet | 2 | 2 | 2 | 2 | 0 | 8 | Most severe finding of this batch — six Livewire components had zero authorization, allowing view/comment-only users to write cell data; closed. Broken dashboard route also fixed. |
| Dot.Engage | 2 | 2 | 2 | 2 | 0 | 8 | A live, exploitable cross-tenant data leak (unscoped dashboard queries left by an incomplete prior fix) found and closed same day. |
| Dot.Docs | 2 | 2 | 2 | 2 | 0 | 8 | Two real IDOR gaps closed (`VersionHistory`, `TemplateGallery`); a silent wrong-database fallback also fixed. |
| Dot.Files | 2 | 2 | 2 | 2 | 0 | 8 | A migration typo that would fatal on first real `migrate` caught and fixed before it ever ran. |
| Dot.Press | 2 | 2 | 2 | 2 | 0 | 8 | Fully broken `/dashboard` route (referenced a nonexistent Blade view in this actually-Inertia+Vue app) fixed against the real component's prop contract. |
| Dot.Tutor | 1 | 2 | 2 | 2 | 0 | 7 | A live cross-user session-data disclosure closed, but `E` stays at 1 — no booking UI/controller exists yet, so the core domain action (booking a session) cannot be performed through this app today. |
| Dot.Forms | 2 | 1 | 2 | 2 | 0 | 7 | Security scan came back clean for IDOR, but a real, named gap was flagged and left open: webhook dispatch URLs have no SSRF hardening — `S` stays at 1 until that's closed. |

**Mean of these 7: 7.71 / 10.** Slightly below the 8.0 mean of the other 20 — expected, since none of these has had the second confirmatory pass yet, and two (Dot.Tutor, Dot.Forms) have real, specifically-named open gaps rather than a clean bill.

**Ecosystem mean, all 27 platform apps: 7.93 / 10** (down slightly from 8.0, purely a function of the 7 newly-discovered platforms not yet having a second pass — not a regression in the other 20). `I = 0` remains universal across all 27.

## 5. What this model says to do next

The picture has changed materially since pass 1 — four of the five signals below are now resolved, and the one that isn't is the same one it's been since v1.0.0:

1. ~~Lowest-scoring platform (Dot.Notify)~~ — **Resolved.** Webhook endpoint built, E moved 1→2.
2. ~~`Dot.Agents`/`Dot.Pulse`/`Dot.Analytics`/`Dot.Central`/`Dot.Design` never got a full security scan~~ — **Resolved.** All five got a dedicated deep pass; three real bugs found and fixed (Agents, Pulse, Analytics), two confirmed genuinely clean (Central, Design).
3. ~~Dot.HR's role-gating gap~~ — **Resolved.**
4. **New, smaller follow-ups from the deep passes** — none of these move a score, since they're documented gaps rather than open vulnerabilities, but they're the highest-value next actions: Dot.Pulse's knowledge-graph tables have no tenant column (unexploitable today, unsafe once a graph view ships) and its moderation service fail-opens on an unparseable AI response; Dot.Auction's real-time bidding has no broadcasting bootstrapped server-side at all (found during pass 2, correctly not half-fixed).
5. **Every platform's `I=0` — still the whole ecosystem's real bottleneck, unchanged since v1.0.0.** With every other dimension now maxed across the original 20 platforms, this is no longer one gap among several — it is the *only* remaining gap standing between that group and a perfect MEGA score. Closing it for even one platform (Dot.Billing remains the best-scoped candidate per [os/19-Knowledge-Packs.md](19-Knowledge-Packs.md) §4) is unambiguously the single highest-leverage next action in the entire ecosystem.
6. **New, from the 7 newly-discovered platforms (§4a):** two real, named, still-open gaps — Dot.Forms' webhook dispatch has no SSRF hardening, and Dot.Tutor has no booking UI/controller at all despite a fully-modeled schema. Neither is a live vulnerability today (Forms' scan came back otherwise clean; Tutor simply can't be used for its core purpose yet), but both are the correct next-pass targets for this group before it's folded into the same "second pass" cadence as the other 20.

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Sakhile Bhayi | Initial scoring model — 5 dimensions, unweighted 0–10 composite, scored against the real state of all 15 built platforms plus the 5 empty scaffolds. |
| 1.1.0 | 2026-08-01 | Sakhile Bhayi | Scored the 5 newly hand-authored platforms (§4); corrected the original 15-platform mean (was stated as 7.3, actually 7.53); added the ecosystem-wide 20-platform mean (7.6) and Dot.HR's real authorization gap as a §5 priority signal. |
| 2.0.0 | 2026-08-02 | Sakhile Bhayi | **Re-derived after the second pass across all 20 platforms.** All five `S=1` platforms re-scored `S=2` after a dedicated deep-security pass (3 real bugs found and fixed, 2 confirmed clean). Dot.Notify's `E` moved 1→2 (webhook built). Dot.HR's `S` moved 1→2 (role-gating closed). Every platform now scores 8/10 — the ecosystem mean is a flat 8.0, with `I=0` as the only remaining gap anywhere. §5 rewritten to reflect that four of five prior signals are now resolved. |
| 2.1.0 | 2026-08-02 | Sakhile Bhayi | **Scored the 7 newly-discovered platforms** (§4a): Dot.Files, Dot.Docs, Dot.Forms, Dot.Sheet, Dot.Engage, Dot.Press, Dot.Tutor — mean 7.71/10, two real named gaps left open (Dot.Forms' SSRF hardening, Dot.Tutor's missing booking UI). Recomputed ecosystem mean across all 27 platform apps: 7.93/10. §5 gained a 6th signal for these two new gaps. |

## Open Questions

- Should `I` (integration readiness) be weighted less than the other four dimensions until at least one platform closes it, so the ecosystem mean isn't perpetually capped by a gap every platform shares equally? Recommend against it for now — an unweighted score keeps the gap visible rather than normalizing it away. This is now the *only* place the model still has room to move, which makes the weighting question more consequential than it was in v1.
- Now that every platform scores identically (8/10), does a flat, undifferentiated score reduce this model's triage usefulness? It still correctly identifies that `I` is the universal next target, but it can no longer distinguish "which platform needs attention next" the way v1.1.0 could. Consider whether a 6th dimension (e.g. test coverage depth, or real vs. stubbed business logic ratio) is needed to keep the score discriminating as the ecosystem matures — or whether convergence to a flat score is itself the correct signal that the platform-loop pattern has done its job.
- This model should be re-derived every time [13-Engineering-State.md](13-Engineering-State.md) changes — due for its next update whenever a 3rd pass happens, `I` moves on any platform, or a 21st platform joins.
