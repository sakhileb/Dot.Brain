---
title: Dot Ecosystem — Design System
version: 1.0.0
status: active
owners: [Sakhile Bhayi]
last-review: 2026-08-01
---

# 06 — Design System

Purpose: document the visual and UX conventions that actually exist across the 15 real Dot platforms touched this session — a descriptive record of what was built and repeated, not an aspirational spec. Where a pattern was applied identically across multiple platforms, it is documented here as the reusable pattern it has become.

> **Related documents:** [os/02-Engineering-Loop.md](02-Engineering-Loop.md) — the process branding/UI passes run within (checklist §5) · [os/07-Development-Standards.md](07-Development-Standards.md) — the code-quality rules governing how these patterns are implemented · [os/13-Engineering-State.md](13-Engineering-State.md) — which platforms have received which pieces of this system · [MANIFESTO.md](../MANIFESTO.md) — principle 4, no placeholder content, applies to UI states as much as to code.

---

## 1. Foundation — Jetstream Teams as the auth/team shell

Every platform in this system that has a real application uses **Laravel Jetstream with the Teams feature** as its authentication and multi-tenancy shell — team switching, team invitations, and team-scoped permissions are the baseline, not an opt-in. This is deliberate: nearly every real authorization bug found this session (see [07-Development-Standards.md](07-Development-Standards.md) §2) was a gap in team-scoping, which means the team boundary is the single most security-relevant surface in the whole design system. Any new screen is built assuming a team context exists and must be checked, not assumed safe.

## 2. Logo system

Each platform has one logo, sourced from a shared asset set (`Dot.logos/` — one PNG per platform, named `dot.<platform>.png`), applied consistently to: the auth pages (login/register), the primary nav bar, and the browser favicon.

The mark itself follows one consistent visual formula across the set:

- A **yellow circle** as the icon base — the constant, ecosystem-wide anchor.
- A **colored arrow** overlaid on the circle, colored per-platform to give each app its own identity within the shared system (this is the one element that varies platform-to-platform).
- A **wordmark**: `dot.<platform>` in lowercase, set next to the mark.

This gives every platform an icon that reads unmistakably as "part of Dot" (the yellow circle is the constant) while remaining visually distinct (the arrow color and wordmark differ). When applying a logo in a branding pass, use the platform's own asset — never substitute another platform's mark or a generic placeholder icon, per the no-placeholder-content rule.

## 3. Dark/light theme toggle

Where a platform's original Jetstream scaffold predates a theme toggle, the pass adds one using a consistent pattern rather than pulling in a new dependency:

- **CSS variables** define the palette for both themes (e.g. `--color-bg`, `--color-surface`, `--color-text`), scoped under `:root` for light and a `.dark` class (or `data-theme="dark"` attribute, depending on which the platform's existing Tailwind config already expects) for dark.
- **Tailwind's `class` dark-mode strategy** (`darkMode: 'class'` in `tailwind.config.js`) drives which variable set applies — not the OS-level `prefers-color-scheme` alone, because the toggle must be a user choice, not just a system default.
- **`localStorage` persistence**: the chosen theme is written to `localStorage` on toggle and read on page load (via a small inline script in the layout head, before Livewire/Alpine hydrate, to avoid a flash of the wrong theme).
- The toggle control itself lives in the primary nav, next to the team switcher — a consistent location across platforms so a user moving between Dot apps finds it in the same place.

This is documented as what actually worked, not a theoretical recommendation: it was the pattern applied wherever a toggle was missing during this session's passes.

## 4. Notification bell pattern

The most heavily reused UI pattern from this session — applied near-identically across roughly ten platforms wherever a real domain event existed to trigger it (per [02-Engineering-Loop.md](02-Engineering-Loop.md) §5, item 3: never added without a genuine trigger).

Architecture:

- **Backend**: Laravel's built-in `database` notification channel. Domain events (a task assigned, an order placed, a bid outbid, etc.) dispatch a `Notification` that implements `toDatabase()`, landing in the standard `notifications` table Jetstream/Laravel already provides.
- **Frontend**: a single Livewire component (a bell icon + unread-count badge + dropdown listing recent notifications, "mark as read" on click, a "mark all as read" action) that polls or listens for updates and renders the current user's unread notifications.
- **Placement**: nav bar, consistently positioned near the team switcher and theme toggle, so the three "always there" controls sit together across every platform.

Because the backend half (the `database` channel + Notification classes) and the frontend half (the Livewire dropdown) are structurally the same shape everywhere, this is documented as a reusable component, not something to re-derive per platform: when adding a bell to a new platform, copy the existing dropdown component's structure and wire it to that platform's own domain events — do not redesign the interaction.

## 5. Empty-state and loading-state conventions

- **Empty states** name the specific thing that's missing and the action to fix it (e.g. "No tasks yet — create your first task" with a button), never a generic "No data" message with no path forward.
- **Loading states** use skeleton placeholders shaped like the content they're replacing (a card skeleton for a card grid, a row skeleton for a table) rather than a generic spinner, wherever the pass touches a view that previously had neither.
- Neither state fabricates content to avoid looking empty — an empty state must look empty, per the no-placeholder-content rule in [07-Development-Standards.md](07-Development-Standards.md) §3.

## 6. What this document does not claim

This document describes only what was actually built and observed across the platforms touched this session (see [13-Engineering-State.md](13-Engineering-State.md) for the current list). It is not a claim that all ~20 Dot platforms currently look like this — the five platforms with no existing app (Dot.Plug, Dot.Farms, Dot.HR, Dot.Dopemine, Dot.Memory) will need these same conventions applied when their Jetstream scaffolds are hand-authored, and that work is unverified until it happens.

---

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | Sakhile Bhayi | Initial design system, documenting patterns actually observed across 15 platforms this session. |

## Open Questions

- Should the notification-bell Livewire component be extracted into a shared package/composer dependency across platforms rather than copy-pasted per repo? Currently copy-pasted; worth revisiting once more platforms need it.
- Is the yellow-circle-plus-colored-arrow mark formal enough to need its own brand guideline doc (color values, arrow-angle rules, minimum size), or is the existing asset set in `Dot.logos/` sufficient as the single source of truth? Leaning toward leaving the PNG set authoritative until a platform needs the mark in a new context (e.g. print, app icon at very small sizes) where the existing raster assets break down.
