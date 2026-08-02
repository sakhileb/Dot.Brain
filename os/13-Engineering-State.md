---
title: Dot Ecosystem — Engineering State
version: 5.0.0
status: active
owners: [Sakhile Bhayi]
last-review: 2026-08-01
---

# 13 — Engineering State

Purpose: a living record of where each Dot platform stands against the [Engineering Loop](02-Engineering-Loop.md) — which platforms extended a real, already-installed Jetstream app versus which were hand-authored from an empty scaffold, and which environment gaps remain open. This document is a snapshot in time, not a fixed spec — it must be updated as platforms get further passes, as environment constraints change, and as gaps close. Treat any date on this document older than a few weeks as a signal to re-verify before relying on it.

> **Related documents:** [os/02-Engineering-Loop.md](02-Engineering-Loop.md) — the process that produced this state · [os/07-Development-Standards.md](07-Development-Standards.md) — standards applied during each pass · [os/06-Design-System.md](06-Design-System.md) — UI patterns applied during each pass · [brain.platforms.md](../brain.platforms.md) — the platform registry this document tracks against.

> **⚠️ Living document.** Update this file every time a platform completes a pass, a new platform is hand-authored, or an environment gap closes. Do not let this drift into an outdated snapshot presented as current state — that would itself violate the no-placeholder-content principle.

---

## 1. Status overview

```mermaid
flowchart LR
    subgraph Extended["15 platforms — extended an existing real app"]
        direction TB
        D1[Dot.Analytics]
        D2[Dot.Pulse]
        D3[Dot.Mines]
        D4[Dot.Notify]
        D5[Dot.Billing]
        D6[Dot.Charts]
        D7[Dot.Emall]
        D8[Dot.Ehail]
        D9[Dot.Agents]
        D10[Dot.Auction]
        D11[Dot.Central]
        D12[Dot.Projects]
        D13[Dot.Tasks]
        D14[Dot.Design]
        D15[Dot.Finance]
    end
    subgraph HandAuthored["5 platforms — hand-authored from an empty scaffold, unverified"]
        direction TB
        E1[Dot.Plug]
        E2[Dot.Farms]
        E3[Dot.HR]
        E4[Dot.Dopemine]
        E5[Dot.Memory]
    end
    subgraph Discovered["7 platforms — pre-built, discovered via InfoDot's registry (§3a)"]
        direction TB
        F1[Dot.Files]
        F2[Dot.Docs]
        F3[Dot.Forms]
        F4[Dot.Sheet]
        F5[Dot.Engage]
        F6[Dot.Press]
        F7[Dot.Tutor]
    end
    HandAuthored -->|"next: real composer install +\nphp artisan migrate/test"| Verified[All 27 — verified in a real environment]
    Extended --> Verified
    Discovered --> Verified
```

All 27 Dot platform apps now have real, committed code as of this update (plus Dot.Brain itself, the ecosystem's intelligence layer). **None of it has been executed** — every platform in all three groups was written and reviewed without a local PHP/Postgres/Docker runtime; see §4. The three groups differ by provenance, not quality: the 15 "extended" platforms started from a real `composer create-project`/`jetstream:install` output and had a domain added or polished on top; the 5 "hand-authored" platforms had their entire Jetstream Teams shell (auth, teams, 2FA, API tokens) copied file-by-file from Dot.Billing's already-real install and adapted, then given a from-scratch domain layer — a materially higher-risk path since no tool ever actually ran `jetstream:install` against these five; the 7 "discovered" platforms (§3a) were already substantially real, working codebases nobody had tracked in Dot.Brain's registry until they were found via InfoDot's own `config/ecosystem.php` and integrated in a single pass each.

Dot.Brain itself (this repository) is out of scope for this loop — it is the ecosystem's intelligence layer, not a Jetstream application, and is governed by its own [CLAUDE.md](../CLAUDE.md) instead.

## 2. Platforms extended from a real, already-installed Jetstream app (15)

Each has had at least one bounded pass per [02-Engineering-Loop.md](02-Engineering-Loop.md) §5 (branding, dark/light toggle, notification bell where a real trigger exists, Feature tests written-but-unexecuted, docs accuracy pass, one security/tech-debt scan). The note below each name is its standout finding from that pass, where a specific issue was identified and fixed; platforms with no specific finding noted had a clean scan at the time of their pass — that is a statement about what was checked, not a guarantee nothing remains.

| Platform | Pass 1 finding | Pass 2 finding |
|---|---|---|
| **Dot.Tasks** | Cross-tenant task access — a controller path loaded tasks by ID without a Policy check against the current team; fixed with team-scoped Policy. | Re-verified: prior fix intact; spot-checked every other component for the same pattern — none found. Clean. |
| **Dot.Emall** | Checkout stock race condition — concurrent checkouts could oversell limited stock; fixed with locking/atomic-decrement. | Dispatched the platform's first real domain event (`OrderPlaced`) from the checkout flow — previously purely conceptual. |
| **Dot.Auction** | Reserve-price leakage — reachable by a bidder before the auction met it; scoped out of the bidder-facing payload (hardened via `$hidden`). | Investigated the missing real-time bid-update wiring; found it's bigger than it looked (broadcasting never bootstrapped server-side at all) — correctly declined a partial fix, flagged as a follow-up task instead. |
| **Dot.Mines** | Unenforced tenant-scoping trait on one model; applied consistently. | Found and fixed a **second, actually-reachable** cross-tenant leak: a user belonging to no team could hit an unscoped `Model::all()` fallback in `ReportController::view2()`, exposing every team's data. Also confirmed a pre-existing Tailwind v4/PostCSS build mismatch, flagged (not fixed — touches the whole build pipeline). |
| **Dot.Finance** | IDOR gaps across finance endpoints; closed with per-record Policies. | Re-verified fixes intact (no Livewire in this app, so the IDOR-via-Livewire-argument pattern doesn't apply); generated the missing `AiInsight` model, replacing a hardcoded `0` with a real (honestly-still-zero) query. |
| **Dot.Charts** | Fixed "AI analysis" demo output was presented as live; relabeled honestly. | Reviewed — nothing else bounded to fix at current maturity; correctly declined to invent work. |
| **Dot.Billing** | Missing invoice-access authorization; closed with a Policy. | Found and fixed a real dead-code bug: wrong config key (`services.anthropic.key` vs. the real `services.anthropic.api_key`) silently disabled the AI spend-insights feature since it was written, with no error surfaced. |
| **Dot.Ehail** | Missing driver-profile authorization; closed with a Policy. | Wired a fully-built `RideCompletedNotification` to a real trigger (ride status → completed) — it existed but nothing had ever called it. |
| **Dot.Agents** | Dead notification bell — fully-built backend, nothing rendered it; wired up. | **Deep security pass on the governance stack (closing the S=1 gap):** found and fixed a real cross-org IDOR — two Livewire components (`ApprovalQueue`, `KnowledgeManager`) looked up records by unchecksummed method arguments, letting any authenticated user read another organization's approval details and knowledge-base content. Delusion-risk scoring, approval-workflow server-side checks, the digital immune system, and the prompt-injection guard all checked out clean. |
| **Dot.Notify** | No inbound webhook endpoint existed at all; flagged, not fixed. | **Built it.** HMAC-SHA256 signature verification, timing-safe comparison, identical generic 401 for unknown-token vs. bad-signature (no enumeration oracle), rate-limited, routes into the existing notification pipeline. |
| **Dot.Pulse** | Feed pagination was missing; added. | **Deep security pass on the moderation pipeline (closing the S=1 gap):** found and fixed a real cross-tenant IDOR across 6 entry points (web, API, Livewire) exposing private-community posts to any authenticated user, plus a moderation fail-open where permanently-failed AI jobs auto-published unmoderated content. Two deeper issues flagged, not fixed: no tenant scoping on the (currently unused) knowledge-graph tables, and a fail-open path inside the moderation service itself on unparseable AI responses. |
| **Dot.Analytics** | Dead duplicate API route removed. | **Deep security pass on the intelligence engines (closing the S=1 gap):** cross-tenant isolation across the 17-engine service, knowledge graph, Business DNA profiling, and reports/briefings all confirmed clean — every query is team-scoped. One real bug found in a more mundane spot: the feature-flags API leaked other teams'/users' IDs to any authenticated user; fixed with role-based field visibility. |
| **Dot.Projects** | Task assignment had no server-side team-membership check; fixed. | Wired the milestone/project status-transition events (`MilestoneReached`, `ProjectClosed`) that existed as enums but were never dispatched. |
| **Dot.Central** | UI parity pass only; no fresh security scan. | **AI-agent domain security review (closing the S=1 gap):** conversation/message/usage-log isolation, the IDOR-via-Livewire-argument pattern, and response caching all checked out clean — genuinely clean, not just "not yet checked." |
| **Dot.Design** | UI parity pass only; no fresh security scan. | **AI canvas-domain security review (closing the S=1 gap):** safe by omission — no per-ID routes/controllers exist yet for Project/Canvas/Asset, so there's no reachable IDOR surface today. Documented for whoever builds that surface next, since it's not tenant-scoped and needs ownership checks from day one. |

## 3. Platforms hand-authored from an empty scaffold (5)

Per [02-Engineering-Loop.md](02-Engineering-Loop.md) §4, each was built by copying the real, already-verified Jetstream Teams shell from Dot.Billing file-by-file (auth, teams, 2FA, API tokens, profile — never re-invented from memory), then adding a from-scratch domain layer. Every one is **explicitly flagged as unverified** in its own commit message and `wiki.md` — none has been installed, migrated, or test-executed.

| Platform | Domain built | Pass 1 characteristic | Pass 2 finding |
|---|---|---|---|
| **Dot.Memory** | Storage-tier/index/retrieval-class/retrieval-observation/durability-outcome telemetry models | "Store without reading" enforced structurally — every column across all 5 tables is a number, enum, or timestamp; backed by `StoreWithoutReadingInvariantTest`. | Re-verified both the access model (correctly ecosystem-wide, not team-scoped) and the invariant — still fully intact. Found and fixed a real UX gap: two real pages (Index Inventory, Durability Outcomes) had no navigation links, reachable only by hand-typing the URL. |
| **Dot.Plug** | Extension/ExtensionVersion/Installation marketplace | Certification gate enforced before install; correctly team-scoped install/uninstall. | Found and fixed a real IDOR: draft/decertified extension listings were readable by any team via direct ID, bypassing the marketplace index's own status filter. |
| **Dot.HR** | Position/Employee/LeaveRequest | Authorization built in from the first commit — but any team member, not just an admin, could mutate employee records. Flagged as the ecosystem's top-priority gap. | **Closed.** `create`/`update`/`delete` on Employee/LeaveRequest/Position now require the team's `admin` role or ownership; `view` stays open team-wide. (The dedicated agent for this hit a session limit mid-task; recovered and completed manually, including the test suite it hadn't finished writing.) |
| **Dot.Dopemine** | Mechanic/MechanicDeployment/ProhibitedMetricPattern catalog | The manifesto's ethics constraint enforced at three structural layers (closed enum, action-level check, model `saving` listener). | Re-verified all three layers still fully intact and unweakened. Found and closed a real test-coverage gap: `MechanicDeployment` tenancy was correctly enforced in code but had no regression test — added one. |
| **Dot.Farms** | Farm/Field/Crop/CropCycle/PlantingRecord/HarvestRecord | `FarmPolicy` verified across every child-resource controller; the one by-ID-route-free resource (Crop) confirmed to have no cross-team surface. | Re-verified — `FarmPolicy` coverage still complete and unweakened across all 6 controllers. Clean. |

## 3a. Seven platforms discovered via InfoDot's registry, integrated 2026-08-02 (7)

These were never hand-authored or extended by this loop — they surfaced when InfoDot's own `config/ecosystem.php` was reconciled against the real ecosystem (see [os/Appendix.md](Appendix.md) §1) and turned out to be real, substantially pre-built Laravel 13/Jetstream 5/Livewire 3 apps (Dot.Press uses Inertia+Vue instead), each already wired with a working `EcosystemAuthController` matching the ecosystem's SSO contract, that nobody had tracked. Each got one integration pass: verify the SSO contract, a bounded IDOR-focused security scan, branding/favicon completion, a `wiki.md`, and a README accuracy correction — the same bar as everything else in this document, just compressed into a single pass since there was no separate "build" phase to do first.

| Platform | Real domain (often didn't match the registry's assumption) | Standout finding |
|---|---|---|
| **Dot.Files** | File/folder manager, single self-referential `objects` table | A migration typo (`contrained` → `constrained`) that would fatal on first real `migrate` — caught before it ever ran. |
| **Dot.Docs** | Real-time collaborative document/wiki (Notion-shaped) | Two real IDOR gaps closed (`VersionHistory`, `TemplateGallery` unscoped lookups); `DB_DATABASE` was silently falling back to a nonexistent database instead of the shared `infodot` instance. |
| **Dot.Forms** | Team-scoped form builder with webhook/CRM dispatch | Security scan came back clean — already properly scoped. SSRF hardening on webhook dispatch URLs flagged 2026-08-02, **closed the same day**: `App\Support\SsrfGuard` rejects loopback/private/link-local URLs at both save and dispatch time, redirects disabled. |
| **Dot.Sheet** | Full spreadsheet platform, real-time collab | **Most severe finding of this batch:** six Livewire components had zero authorization checks, and the main grid only checked `view` at mount without re-checking `update` on mutations — view/comment-only shared users could actually write cell data, comments, and charts. Closed. Also fixed a broken `/dashboard` route left inconsistent by a prior commit. |
| **Dot.Engage** | Contract sharing, chat & video-call document signing — **not** the marketing/campaign product its `campaign` registry icon implied | **A live, exploitable cross-tenant data leak**, not just a theoretical gap: an incomplete prior "fix" commit added dashboard queries with zero team scoping, so every team's dashboard showed every other team's actual contract/conversation records. Closed same day found. |
| **Dot.Press** | Slide-deck design tool (Canva-shaped) — **not** the newsroom/CMS its `newspaper` registry icon implied; also the one platform on Inertia+Vue instead of Livewire | A fully broken `/dashboard` route (referenced a Blade view that doesn't exist in this Inertia app) — fixed by reading the real Vue component's expected props, not guessed. |
| **Dot.Tutor** | Tutoring marketplace — schema + dashboard only, no booking UI existed | A live cross-user data disclosure: any logged-in user could see every other user's session pairing/subject/dollar amount on `/dashboard`. Closed. Also correctly overrode an incorrect assumption in its own task brief about a stray logo file being a personal brand mark — verified directly, found it was genuinely the platform's real logo. **The missing booking UI itself was built the same day**: `TutorBookingController` + `TutorSessionPolicy` (authorization designed in from the start, not retrofitted) — a student can now actually browse tutors and book a session through this app. |

Formally registered in [brain.platforms.md](../brain.platforms.md) with their own `platforms/*.md` docs (2026-08-02) — the ecosystem is now 28 product platforms, not 20.

## 4. Known outstanding environment gaps

1. **No local PHP, PostgreSQL, or Docker — RESOLVED 2026-08-02 for the environment itself; execution across the remaining 24 platforms is still outstanding.** Homebrew, PHP 8.3 and 8.5 (different platforms pin incompatible ranges — see §4a), Composer, and PostgreSQL 16 are now installed and working. Three platforms (Dot.Billing, Dot.Forms, Dot.Tutor) have been actually run — `composer install`, `migrate`, and the full test suite — for the first time this entire session. See §4a for what that found. The other 24 platforms (including all 5 hand-authored ones) have still never been executed; that remains the largest concrete next step, and it is a strictly bigger risk for the 5 hand-authored platforms in §3 than for platforms extended from a real install.
2. **Branch protection on Dot.Brain's `main` is being bypassed via admin override, not a real PR flow.** Commits to this repository are currently landing directly on `main` through an administrator bypass rather than going through pull-request review, which is the intended flow per [CLAUDE.md](../CLAUDE.md)'s non-negotiable rules on auditability. This should be corrected — either by routing future changes through actual PRs, or by explicitly documenting why the bypass is temporarily acceptable and for how long.
3. **NEW, found by §4a: the "shared `infodot` database" convention as currently implemented cannot actually support more than one platform.** Every platform's `.env.example` points `DB_DATABASE` at the same `infodot` database, and every platform ships its own full, independent Jetstream migration set (its own `create_users_table`, `create_teams_table`, `add_two_factor_columns_to_users_table`, etc.). The first platform to migrate against a real `infodot` database succeeds; every platform after it collides — confirmed directly: after Dot.Billing's migrations ran, Dot.Forms' migration `add_two_factor_columns_to_users_table` failed with `column "two_factor_secret" of relation "users" already exists`. This was invisible for the entire session because nothing had ever actually run two platforms' migrations against the same database before today. It needs an architectural decision — separate databases per platform (breaking the documented SSO/shared-DB convention), a shared schema owned by exactly one platform's migrations with others treated as read-only consumers, or per-platform PostgreSQL schemas within one database — not a quick patch. Flagged here rather than fixed; see Open Questions.

## 4a. First real execution results (2026-08-02)

Ran for real, for the first time this session, against a fresh Homebrew-installed PHP/Composer/PostgreSQL 16 toolchain (see §4 item 1):

| Platform | `composer install` | `migrate` | Tests | Real bugs found by executing (not by review) |
|---|---|---|---|---|
| Dot.Billing | Clean (PHP 8.5, satisfies `^8.3`) | Clean, 10 migrations | 65 total, 58 passed, 7 skipped by config, 0 failed | `config/dkp.php`'s `env('DKP_SIGNING_KEY_PATH', default)` returned the blank string `.env.example` ships rather than falling through to the default — Laravel's `env()` doesn't treat a set-but-empty variable as unset. `dkp:publish-metric` failed immediately. Fixed to `env(...) ?: default`; re-ran, produced a real signed pack, independently re-verified outside the PHP process against the committed public key. |
| Dot.Forms | **Failed on PHP 8.5** — `phpoffice/phpspreadsheet` 1.30.2 (locked) requires `php <8.5.0`; had to install PHP 8.3 specifically and use it explicitly. Clean once switched. | Clean, 19 migrations, **after a fix** | 33 total, 30 passed, 3 skipped by config, 0 failed | `create_form_fields_table` and `create_forms_table` shared the identical timestamp `2026_04_02_062015`; Laravel breaks ties alphabetically by filename, and `form_fields` sorts before `forms_table`, so the FK-dependent migration ran first and failed with `relation "forms" does not exist`. This would have fatally broken the very first `php artisan migrate` on a genuinely fresh install. Fixed by renaming `create_forms_table`'s file to one second earlier. Also directly exercised `SsrfGuard::isSafeUrl()` against 8 real cases (public host, loopback, localhost, the `169.254.169.254` cloud metadata endpoint, two private ranges, non-http(s) scheme, public IP) — all passed. |
| Dot.Tutor | Clean (PHP 8.5) — **note:** Dot.Tutor's lock file requires `php >=8.4.1` (Symfony 8.1), the opposite constraint direction from Dot.Forms above; the two platforms are not compatible with the same PHP minor version simultaneously without keeping multiple PHP installs on the same machine, which this environment now has (8.3 and 8.5) | Clean, 9 migrations | 52 total, 45 passed, 7 skipped by config, 0 failed | No bug in the booking flow itself — added 6 new Feature tests (`TutorBookingTest`) covering the browse/book/self-booking-rejection/subject-mismatch-rejection/view-authorization/cancel paths built in the previous pass; all 6 passed on first run, a genuine validation that the hand-authored controller and policy work as written, not just that they compile. |

Every platform was verified against its own isolated database (`dot_forms_verify`, `dot_tutor_verify`), not the shared `infodot` one, specifically because of the item-3 collision above — using the shared database would have made it impossible to tell "my migration is broken" apart from "a different platform already claimed this table."

## 5. How to update this document

All 20 platforms now have code; there is no longer an "empty" category to promote platforms out of. Going forward: when a platform gets a further pass, update its row in §2 or §3 with the new finding. When §4's environment gaps close, move the corresponding item into the change log below with the date it closed, and remove it from the open list. If a 21st platform is ever added to the ecosystem, give it its own row in whichever of §2/§3 fits once it has code, per the pattern already established here.

---

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Sakhile Bhayi | Initial state snapshot — 15 platforms through the loop, 5 empty scaffolds, 2 open environment gaps. |
| 2.0.0 | 2026-08-01 | Sakhile Bhayi | All 5 previously-empty platforms (Dot.Plug, Dot.Farms, Dot.HR, Dot.Dopemine, Dot.Memory) hand-authored and pushed. §3 rewritten from "not started" to a completion table with each platform's standout characteristic. All 20 Dot platforms now have real code; the "empty scaffold" category no longer exists. |
| 3.0.0 | 2026-08-02 | Sakhile Bhayi | **Second pass across all 20 platforms**, run in 4 batches with periodic check-ins. §2/§3 tables restructured to show pass-1 vs. pass-2 findings side by side. Found and fixed **8 more real bugs**, including two serious cross-tenant/cross-org IDOR vulnerabilities (Dot.Agents' governance stack, Dot.Pulse's private communities) and a second, actually-reachable cross-tenant leak in mines (distinct from pass 1's fix). Closed the `S=1` gap on all 5 previously-fenced-off platforms (Dot.Agents, Dot.Pulse, Dot.Analytics, Dot.Central, Dot.Design) with a dedicated deep pass on exactly the internals pass 1 declined to touch. Closed Dot.HR's top-priority role-gating gap. Dot.Auction and ChartSense correctly declined to force a fix where investigation showed the real gap was bigger than it looked, or nothing bounded existed. |
| 4.0.0 | 2026-08-02 | Sakhile Bhayi | **Seven previously-untracked platforms discovered and integrated** (§3a): Dot.Files, Dot.Docs, Dot.Forms, Dot.Sheet, Dot.Engage, Dot.Press, Dot.Tutor — found via reconciling InfoDot's own `config/ecosystem.php` against the real ecosystem, each already a substantially real, working codebase nobody had registered. Closed two live, exploitable cross-tenant/cross-user data leaks (Dot.Engage's dashboard, Dot.Tutor's dashboard), a systemic Livewire authorization gap allowing view-only users to write data (Dot.Sheet), two broken dashboard routes (Dot.Sheet, Dot.Press), two IDOR gaps (Dot.Docs), and a migration typo that would fatal on first real `migrate` (Dot.Files). §1 status diagram now shows all three provenance groups (Extended/HandAuthored/Discovered) totaling 27 platform apps. Formally registered in [brain.platforms.md](../brain.platforms.md) — the ecosystem is now 28 of 28 platforms registered. |
| 4.1.0 | 2026-08-02 | Sakhile Bhayi | **Second pass on the two §3a platforms with real, named open gaps.** Dot.Forms' SSRF gap closed (`App\Support\SsrfGuard`, enforced at both save and dispatch time). Dot.Tutor's missing booking flow built (`TutorBookingController` + `TutorSessionPolicy`, browse/book/view/cancel a session, authorization designed in from the start). §3a table updated for both. Also: Dot.Billing became the ecosystem's first real DKP onboarding-step-1 completion the same day — see [os/19-Knowledge-Packs.md](19-Knowledge-Packs.md) §4a, not duplicated here since it's a cross-cutting protocol milestone, not a platform-domain finding. |
| 5.0.0 | 2026-08-02 | Sakhile Bhayi | **§4 item 1 (no execution environment) closed for real, for the first time this entire session.** Installed Homebrew, PHP 8.3 + 8.5, Composer, PostgreSQL 16. Ran `composer install && migrate && test` for real against Dot.Billing, Dot.Forms, and Dot.Tutor — new §4a has the full results. Found and fixed 2 real, previously-invisible bugs (Dot.Billing's `env()` blank-string config bug, Dot.Forms' migration-ordering bug that would have fatally broken a fresh install) and added 6 genuinely-executed regression tests for Dot.Tutor's booking flow, all passing. Also discovered and flagged (not fixed) a significant new architectural gap: the shared-`infodot`-database convention breaks the moment a second platform's independent Jetstream migrations run against it — new §4 item 3. |

## Open Questions

- **Resolved:** the Central/Design re-scan question from v2.0.0 — both got a dedicated pass this round; both are clean.
- **Resolved:** Dot.HR's role-gating gap — closed this round.
- What is the target date or trigger condition for closing the branch-protection bypass on Dot.Brain's `main` (§4, item 2)? Not yet decided — needs an owner and a deadline.
- **Partially resolved:** a real PHP/Composer/PostgreSQL environment now exists (§4a) — but the 5 hand-authored platforms specifically (§3) still haven't had `composer install` run against their copied `composer.lock`. That remains the single highest-risk unexecuted code in the ecosystem and should be the next verification target.
- **New, urgent:** how should the shared-`infodot`-database collision (§4 item 3) actually be resolved? Three options sketched there (per-platform databases, single-owner shared schema, per-platform PostgreSQL schemas) — none evaluated yet. This blocks ever running two platforms' migrations against the same real database, which is what the ecosystem's own SSO/shared-DB documentation assumes happens in production.
- **New:** different platforms pin incompatible PHP version ranges (Dot.Forms needs `<8.5`, Dot.Tutor needs `>=8.4.1`) — confirmed directly in §4a. Should the ecosystem standardize on one PHP minor version across all platforms, or is running multiple PHP installs side-by-side (as this verification pass did) an acceptable permanent shape for a polyglot-by-`composer.lock` ecosystem?
- Dot.Pulse's two deep-pass findings that were flagged but not fixed (knowledge-graph tenant scoping, moderation-service fail-open on unparseable AI responses) and Dot.Auction's full broadcasting gap (Reverb never bootstrapped server-side) are the highest-value named follow-ups now that the broader sweep is done — should one of these be the next dedicated pass?
