# 05 — Platform Integrator

> **Prerequisite:** `00-System-Prompt.md` loaded; DKP from `02` and repository from `01` exist.
> **Single responsibility:** Connect every Dot platform to Dot.Brain — one platform per session — and define the universal integration path for future platforms.
> **Invocation:** "Using 05, integrate `Dot.<Platform>`."

---

## TASK

For the named platform, produce its complete integration package. Work from the platform definitions in `00-System-Prompt.md` and the DKP spec. One platform per session; run `Dot.Platforms (universal)` first if `brain.platforms.md` does not yet exist.

## INTEGRATION PACKAGE (per platform)

1. **`platforms/<platform>.md`** — from the platform-knowledge template:
   - Purpose and business domain.
   - **Entities owned** (mapped to knowledge-graph node types) — e.g., Dot.Mines: machines, shifts, pits, haul cycles; Dot.Charts: instruments, strategies, backtests, signals, journals; Dot.Ehail: fleets, drivers, trips, operator businesses.
   - **Events emitted** (mapped to the event taxonomy in `brain.events.md`).
   - **Knowledge Packs published** — which DKP payload types, at what cadence, with example pack IDs.
   - **Intelligence consumed** — which recommendation types the platform subscribes to, with the metric each is expected to move.
   - **Cross-platform relationships** — explicit edges to other platforms (e.g., Dot.Mines ↔ Dot.Central operational loop; Dot.Farms → Dot.Emall → Dot.Billing value chain; Dot.Pulse discussions → Dot.Brain insights; Dot.Dopemine engagement signals ← all platforms).
   - **Tenancy model** — how multi-tenant data stays isolated through ingestion and reasoning.
   - **Dopamine surface** — which user progress/achievement signals it shares with Dot.Dopemine, and the ethical constraints.
2. **`platform.dkp.json` manifest example** — registration file: identity, signing key reference, published pack types, subscribed recommendation types, schema versions.
3. **One fully worked round-trip** — a realistic Knowledge Pack from this platform → Dot.Brain validation → graph updates (list the nodes/edges created) → a generated PR back to the platform (or to a sibling platform) with rationale, confidence, and impact declarations.
4. **Integration status entry** for `brain.platforms.md` — status, DKP version, trust score baseline, open gaps.

## UNIVERSAL FUTURE-PLATFORM PATH

In the `brain.platforms.md` session, define the invariant onboarding procedure:

1. Author `platform.dkp.json` and sign it.
2. Register with the platform registry (automated validation).
3. Drop `platforms/<new>.md` from the template.
4. Publish a first "hello" Knowledge Pack (entity model + event model).
5. Dot.Brain auto-creates graph namespaces and baseline relationships.
6. Trust score starts at probationary level; grows with validated packs.

**Invariant:** steps never change regardless of the platform's domain. If a new platform can't onboard this way, file an ADR — the architecture, not the platform, is at fault.

## EXIT CRITERIA (per platform)

The platform team could implement their DKP publisher this sprint using only the integration package; the worked round-trip example is realistic enough to be a test fixture.
