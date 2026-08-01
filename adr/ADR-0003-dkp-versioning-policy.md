---
title: ADR-0003 — DKP Versioning Policy
version: 1.0.0
status: active
owners: [Chief Knowledge Engineer, Architecture Agent]
last-review: 2026-08-01
---

# ADR-0003 — DKP Versioning Policy: Dual Semver with N−1 Support and `x-` Extensions

Purpose: record the versioning and compatibility policy for the DKP protocol and Knowledge Packs.

> **Related documents:** [../brain.dkp.md](../brain.dkp.md) §2 · [ADR-0002](ADR-0002-dkp-signature-scheme.md)

## Status
Accepted — 2026-08-01

## Context
Twenty-one platforms (and unknown future ones) will implement DKP on independent release cadences. The protocol must evolve without coordinated flag-days, and packs themselves need content versioning independent of protocol versioning.

## Decision
1. **Dual semver:** `dkp_version` (protocol, owned by Dot.Brain) and `pack_version` (content, owned by publisher) are independent.
2. **N−1 MAJOR support:** Dot.Brain ingests packs from the previous protocol MAJOR via versioned translation adapters; translations are appended to provenance.
3. **18-month sunset window** per MAJOR, with escalating warnings in the final 6 months, then `DKP_VERSION_SUNSET` rejection.
4. **`x-` extension fields** everywhere: validators MUST ignore unknown `x-*` fields. Proven extensions are promoted at the next MINOR; the `x-` form stays accepted until the next MAJOR.

## Alternatives Considered
| Alternative | Why rejected |
|---|---|
| Single version for protocol + content | Conflates publisher assertions with wire evolution; forces meaningless version bumps |
| Date-based versions (e.g., `2026-08`) | No compatibility semantics; consumers can't infer breakage from the number |
| Support all past MAJORs forever | Adapter matrix grows unboundedly; contradicts 20-year maintainability |
| Strict validation (reject unknown fields, no extensions) | Kills forward compatibility; every experiment would require a protocol release |

## Consequences
**Positive:** platforms upgrade on their own schedule within a bounded window; experimentation is free via `x-`; provenance records every translation. **Negative:** adapters are code that must be tested against golden packs; `x-` fields are unvalidated by definition, so they never carry trust-scored assertions (rule: assertions in `x-` fields do not enter the graph).

## Change Log
| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | DKP Architect (prompt 02) | Initial decision |

## Open Questions
- Should the sunset window be configurable per platform tier (e.g., 24 months for regulated industries like Dot.Finance)?
