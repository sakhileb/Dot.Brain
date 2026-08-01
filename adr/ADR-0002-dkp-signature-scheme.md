---
title: ADR-0002 — DKP Signature Scheme
version: 1.0.0
status: active
owners: [Security Officer, Security Agent]
last-review: 2026-08-01
---

# ADR-0002 — DKP Signature Scheme: Ed25519 over JCS-Canonicalized JSON

Purpose: record the choice of digital signature scheme for Knowledge Packs.

> **Related documents:** [../brain.dkp.md](../brain.dkp.md) §5 · [ADR-0003](ADR-0003-dkp-versioning-policy.md)

## Status
Accepted — 2026-08-01

## Context
Every pack must be verifiably attributable to a platform and its contributors (human and AI) for 20+ years, across heterogeneous implementations, with zero-downtime key rotation. Signatures must survive JSON re-serialization differences between languages.

## Decision
- **Ed25519** signatures over the **RFC 8785 (JSON Canonicalization Scheme)** form of the envelope, excluding the `signatures` array.
- Multiple detached signatures per pack (platform mandatory; contributors as applicable), each `{key_id, algorithm: "ed25519-jcs", signed_at, value}`.
- Keys registered in `platform.dkp.json` / contributor registry with `valid_from`/`valid_to` overlap windows for rotation; verification time is recorded so revocation is never retroactive.

## Alternatives Considered
| Alternative | Why rejected |
|---|---|
| RSA-PSS / ECDSA P-256 | Larger keys/signatures, nonce fragility (ECDSA), no benefit over Ed25519 for this threat model |
| JWS (RFC 7515) wrapping | Base64url-wraps the payload, making packs unreadable at rest and diff-hostile in PRs and audits |
| Whole-file hash + external signature manifest | Splits trust artifact from knowledge artifact; drifts over 20-year horizons |
| No canonicalization (sign raw bytes) | Breaks the moment any intermediary re-serializes; unacceptable for a multi-language ecosystem |

## Consequences
**Positive:** small, fast, deterministic signatures; packs stay human-readable; multi-signer supports human+AI accountability; rotation without downtime. **Negative:** JCS canonicalizer required in every SDK (well-specified, widely implemented); Ed25519 migration path to post-quantum schemes must be revisited — trigger: NIST PQC signature standard reaching broad library support (tracked as open question).

## Change Log
| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | DKP Architect (prompt 02) | Initial decision |

## Open Questions
- Post-quantum readiness: dual-sign (Ed25519 + ML-DSA) trial window to be scheduled by Security Agent.
