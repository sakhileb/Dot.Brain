---
title: ADR-0013 — Idempotent Shared Jetstream Migrations
version: 1.0.0
status: active
owners: [Chief Architect, Sakhile Bhayi]
last-review: 2026-08-02
---

# ADR-0013 — Idempotent Shared Jetstream Migrations

Purpose: record the decision on how every Dot platform's Jetstream-core migrations (`users`, `teams`, `team_user`, `team_invitations`, `personal_access_tokens`, two-factor columns) should behave against the ecosystem's shared `infodot` database, after actually running two platforms' migrations against it for the first time exposed that they collide.

> **Related documents:** [os/13-Engineering-State.md](../os/13-Engineering-State.md) §4 item 3 — the finding this ADR resolves · [os/05-Knowledge-Protocol.md](../os/05-Knowledge-Protocol.md) — the SSO contract that requires a genuinely shared `users`/`personal_access_tokens` table · [os/02-Engineering-Loop.md](../os/02-Engineering-Loop.md) — the bounded-pass discipline this fix follows

---

## Status

Accepted — 2026-08-02

## Context

Every Dot platform's ecosystem SSO works by one shared mechanism: a `PersonalAccessToken` minted in one platform's session must be verifiable — same row, same database — by every other platform's `EcosystemAuthController`. That only works if `users`, `teams`, `team_user`, `team_invitations`, and `personal_access_tokens` are the *same physical tables*, not per-platform copies. Every platform's `.env.example` already points `DB_DATABASE` at the same `infodot` database, consistent with that design.

What nobody had verified — because no platform had ever actually been executed against a real database before 2026-08-02 (see [os/13-Engineering-State.md](../os/13-Engineering-State.md) §4a) — is that every platform also ships its **own independent copy** of the Jetstream-install migrations that create those same tables. The first platform to migrate against a real `infodot` database succeeds. The second collides: confirmed directly when Dot.Forms' `add_two_factor_columns_to_users_table` migration failed with `column "two_factor_secret" of relation "users" already exists`, because Dot.Billing's identical migration had already run.

## Decision

**Make every platform's copy of the six shared Jetstream-core migrations idempotent — safe to run whether or not another platform already created the underlying tables/columns — rather than designating one platform as the sole owner of those tables.**

Concretely, for each of:
- `create_users_table` (and its cache/jobs siblings, which are not actually shared data but ship in the same migration file in most platforms)
- `add_two_factor_columns_to_users_table`
- `create_personal_access_tokens_table`
- `create_teams_table`
- `create_team_user_table`
- `create_team_invitations_table`

wrap the `up()` method: `Schema::create(...)` calls become `if (! Schema::hasTable('x')) { Schema::create(...); }`, and `Schema::table(...)->addColumn` calls become guarded per-column with `if (! Schema::hasColumn('x', 'y'))`.

This was chosen over the two alternatives below because it requires **no cross-platform coordination or ordering** — any platform can migrate first, in any order, against a shared or an isolated database, and the result is identical either way. That property matters specifically for this ecosystem, where platforms are engineered and deployed independently by design (per [CLAUDE.md](../CLAUDE.md)'s "every platform remains autonomous" principle) — an owner-platform-must-migrate-first rule would silently reintroduce the coupling the ecosystem's architecture exists to avoid.

## Consequences

- **Positive:** any platform can be migrated first or last against a shared `infodot` database with no special procedure. Verification (§ below) confirms this against real Postgres, not just in theory.
- **Positive:** the fix is entirely local to each platform's own repository — no shared migration package, no new coordination service, no change to the SSO contract itself.
- **Negative:** the six shared migrations are now duplicated *and* guarded across every platform's repo rather than owned once — a schema change to, say, `personal_access_tokens` (e.g. a new column every platform needs) still has to be hand-applied to every platform's copy, guards and all. This ADR does not solve that; it only makes the current duplication safe to co-execute, not less duplicated.
- **Negative:** an out-of-order guard bug is possible if a platform's shared-table migration does something *non-additive* (a `DROP COLUMN`, a type change) — none of the six do today, but this pattern only stays safe if every platform's shared migrations stay strictly additive going forward. Flagged as a convention future migrations must follow, not just this pass.
- **Verified 2026-08-02:** applied to Dot.Billing, Dot.Forms, and Dot.Tutor — the three platforms with a working local toolchain — then actually ran all three platforms' full migration sets back-to-back against one real, shared `infodot` database (dropped and recreated clean first). All three completed with zero errors, and each platform's Feature test suite still passed afterward. See the change log entries in each platform's `wiki.md`.

## Alternatives Considered

| Alternative | Why rejected |
|---|---|
| **Designate one platform (e.g. InfoDot) as the sole owner of the shared tables; every other platform deletes its copies of the six migrations and assumes they already exist.** | Technically cleaner (no duplication), but reintroduces exactly the deploy-ordering coupling the autonomous-platform principle exists to avoid — every other platform's `migrate` would hard-fail on a fresh database unless InfoDot's migrations had already run first, in the right environment, by someone who knew the rule. That's a footgun for a solo operator managing 27 repos with no shared CI today. |
| **Per-platform PostgreSQL schemas (namespaces) within one database.** | Doesn't actually solve the problem — it isolates each platform's `users` table into its own namespace, meaning they'd no longer be the same rows at all, breaking the SSO contract's core assumption (a token minted by one platform must be readable by another) rather than fixing the collision. |
| **Separate database per platform.** | Same fatal flaw as the schema-namespace option — breaks SSO by construction, not just as an edge case. Would require redesigning the ecosystem's entire authentication model, which is far outside the scope of fixing a migration-ordering collision. |

## Rollout

Applied and verified for Dot.Billing, Dot.Forms, and Dot.Tutor in this pass. The remaining 24 platforms still have the un-guarded, colliding version of these six migrations — each one is safe running alone (against its own isolated database, which is how every platform has been verified so far per [os/13-Engineering-State.md](../os/13-Engineering-State.md) §4a) but will collide the moment two of them run against the same real `infodot` database, exactly as Forms and Billing did before this fix. Applying the same guard pattern to the other 24 is tracked as an open item, not done in this pass — each platform's migrations differ slightly (different filenames/timestamps), so it is not a single mechanical find-and-replace across the whole ecosystem.

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-02 | Sakhile Bhayi | Initial decision record — idempotent shared migrations chosen over single-owner or per-platform-database alternatives, verified against 3 platforms running back-to-back on one real shared database. |

## Open Questions

- Should the six shared migrations be extracted into a proper shared Composer package (e.g. `dot-ecosystem/jetstream-core-migrations`) that every platform depends on, rather than each platform hand-maintaining its own guarded copy? Would solve the "changed once, applied 27 times by hand" problem this ADR's consequences section flags, at the cost of a new shared-package release/versioning process this ecosystem doesn't have yet.
- Who applies this same guard pattern to the remaining 24 platforms, and in what order? Not yet scheduled.
- Does this pattern need a regression test that specifically runs two platforms' migrations back-to-back against one database, so a future non-additive shared-table migration is caught before it breaks co-execution? None exists yet — this pass verified manually.
