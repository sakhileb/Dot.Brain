---
title: Dot Ecosystem — Development Standards
version: 1.0.0
status: active
owners: [Sakhile Bhayi]
last-review: 2026-08-01
---

# 07 — Development Standards

Purpose: the coding standards actually observed while executing the [Engineering Loop](02-Engineering-Loop.md) across 15 real Dot platforms this session — not an aspirational style guide, but a record of the conventions that were followed and should keep being followed. Where a rule cites a specific fix made this session, that fix is the canonical example to point to when explaining the rule.

> **Related documents:** [os/02-Engineering-Loop.md](02-Engineering-Loop.md) — the process this document's standards apply within · [os/06-Design-System.md](06-Design-System.md) — visual/UX conventions (the front-end half of these standards) · [os/13-Engineering-State.md](13-Engineering-State.md) — which platforms these standards have actually been applied to · [MANIFESTO.md](../MANIFESTO.md) — principle 4, "no placeholder content," is the root of §3 below.

---

## 1. Language and framework conventions

- **PHP**: PSR-12 formatting, strict types where the surrounding file already uses them (match, don't introduce inconsistently). Constructor property promotion for new classes. Avoid static facades inside domain logic where dependency injection is already the file's pattern.
- **Eloquent**: prefer relationship methods over raw queries for anything crossing model boundaries. Scope queries (`Model::query()->where(...)`) over manual SQL. Use `casts()` (Laravel 12 style) rather than the legacy `$casts` property when touching a model that has already been migrated to it — do not mix both styles in one file.
- **Livewire**: components own their own validation rules; do not duplicate validation between a Livewire component and a Form Request unless the same input is genuinely reachable through both paths.
- **Blade**: components over partials for anything reused more than twice (see the notification-bell pattern in [06-Design-System.md](06-Design-System.md) §4, which is exactly this — one Blade/Livewire component reused near-identically across ~10 platforms).

## 2. Authorization — Policy-based, by default

Every one of the real vulnerabilities found this session (Dot.Tasks cross-tenant task access, Dot.Finance IDOR gaps, Dot.Mines' unenforced tenant-scoping trait) was an authorization gap. The standard going forward is not "remember to check ownership" — it's **Laravel Policies as the default mechanism for any per-record access control**, because a Policy is testable, centralized, and visible in one place instead of scattered across controller methods.

The two canonical shapes, observed repeatedly across the 15 platforms:

```php
// Team-scoped platforms (Jetstream Teams is the tenancy boundary)
public function view(User $user, Task $task): bool
{
    return $user->belongsToTeam($task->team);
}

public function update(User $user, Task $task): bool
{
    return $user->belongsToTeam($task->team)
        && $user->hasTeamPermission($task->team, 'tasks:update');
}
```

```php
// Single-user-scoped platforms (no team boundary on this resource)
public function view(User $user, Invoice $invoice): bool
{
    return $user->id === $invoice->user_id;
}
```

Rules:

- Every controller action that loads a model by ID and is reachable by more than one user **must** authorize via a Policy (`$this->authorize(...)` or route-model-binding + Policy) — never rely on the query alone (`Team::find($id)->tasks` style queries that never check whether the *current* user's team matches are exactly how the Dot.Tasks and Dot.Finance gaps happened).
- Do not roll a custom `if ($model->team_id !== auth()->user()->current_team_id)` check inline in a controller. If it's worth checking, it's worth a Policy method, because inline checks are what get forgotten on the next endpoint (this is precisely how Dot.Mines' tenant-scoping trait ended up unenforced on one code path while correctly enforced elsewhere).
- New resources get a Policy created and registered at the same time the model and controller are created — not retrofitted later.

## 3. No placeholder content, no faked output

This is the single most load-bearing rule in this document, inherited directly from Manifesto principle 4. It applies to production code, not just documentation.

**Canonical example**: Dot.Charts' "ChartSense" feature previously presented an AI market analysis result to users without disclosing that the analysis was a fixed demo output, not a live inference. That is exactly the failure mode this rule exists to prevent — a human-facing artifact claiming to be real when it isn't, with no way for the user to tell the difference. The fix was not to delete the feature; it was to **honestly label it as a demo** wherever it renders, so the claim matches the reality.

Applying this generally:

- Any UI element that shows AI-generated, computed, or "smart" output must be backed by a real computation, or must say plainly that it is a demo/placeholder — never one presented as the other.
- Seed/demo data used in local development must be visually or textually distinguishable from what a production tenant would see; it must never leak into a real workflow silently.
- A stubbed integration (e.g. a payment gateway sandbox) is fine; a stubbed integration *pretending* to be live is not.
- If a feature genuinely cannot be completed in a bounded pass (see [02-Engineering-Loop.md](02-Engineering-Loop.md) §5), leave it visibly unfinished (a documented TODO, a disabled UI state) rather than fake the finished behavior.

## 4. Test-writing standards (even when tests cannot be executed locally)

Per [02-Engineering-Loop.md](02-Engineering-Loop.md) §2, this environment has no PHP/Postgres, so every Feature test written this session was **written but not run**. That constraint does not lower the bar for what "written" means:

- Use Laravel's Feature test conventions (`tests/Feature/...`), not Unit tests, for anything touching HTTP, auth, or the database — most of what this loop changes.
- Every new Policy gets a corresponding authorization test: at minimum, one case proving access is granted correctly and one proving it is denied correctly (the denial case is the one that would have caught the Dot.Tasks and Dot.Finance gaps before merge).
- Use factories, not hand-built arrays, for model state — match whatever factory conventions already exist in the target repo.
- Name tests for the behavior, not the method: `test_user_cannot_view_another_teams_task()`, not `test_view_task()`.
- Every commit that includes unexecuted tests states so explicitly in the commit message (see §5) so a human reviewer and CI both know execution status has not been verified.

## 5. Commit message conventions

- Present tense, imperative mood: "Add notification bell to Dot.Pulse", not "Added" or "Adds".
- One platform, one concern per commit where practical — the bounded-pass checklist in [02-Engineering-Loop.md](02-Engineering-Loop.md) §5 maps naturally to a small number of commits per pass, not one giant commit.
- Security-adjacent fixes state the vulnerability class in the message, not just the fix ("Fix cross-tenant task access via missing Policy check on TaskController@show", not "Fix bug in tasks").
- Any commit containing unexecuted tests or an unverified hand-authored scaffold (per [02-Engineering-Loop.md](02-Engineering-Loop.md) §4) says so in the body, e.g. "Tests written, not executed — no local PHP/Postgres available."

---

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Sakhile Bhayi | Initial standards, extracted from real conventions observed across 15 platforms this session. |

## Open Questions

- Should a static-analysis tool (Larastan/PHPStan) be adopted as a standard gate once a real PHP environment is available, or left to each platform's own CI? Not yet decided.
- Should the Policy-pattern examples in §2 be promoted into a shared trait/base Policy class across platforms, or kept as a convention each platform re-implements? Current state is the latter; worth revisiting once more platforms are past their first pass.
