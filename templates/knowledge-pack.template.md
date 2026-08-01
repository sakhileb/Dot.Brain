---
title: Knowledge Pack Template
version: 1.0.0
status: active
owners: [Chief Knowledge Engineer, Repository Steward Agent]
last-review: 2026-08-01
---

# Knowledge Pack Template

Purpose: the human-readable companion for authoring a DKP Knowledge Pack. The wire format is JSON validated by [../schemas/knowledge-pack.schema.json](../schemas/knowledge-pack.schema.json); this template guides content quality before serialization.

> **Related documents:** [../brain.dkp.md](../brain.dkp.md) · worked example: [knowledge-pack.example.md](knowledge-pack.example.md)

---

## 1. Envelope

- **Pack ID:** `dkp:<platform>:<uuid>` (generate once, never reuse)
- **Pack version:** start at `1.0.0`; MAJOR only when reversing an assertion
- **Title:** ≤ 200 chars, states the knowledge, not the activity ("Haul-truck cycle time rises 12% after shift change", not "Q3 analysis")
- **Summary:** one paragraph a non-specialist can act on
- **Supersedes:** pack ID being replaced, if any

## 2. Contributors

List every human and AI that materially shaped the pack. `kind` is permanent. Each contributor's `key_id` must exist in the contributor registry.

## 3. Payloads (≥ 1)

Pick the correct `payload_type` (entity_model | event_model | workflow_model | metric | insight | recommendation | document | discussion | learning_history | incident_report). Checklist per type:

- **insight** — statement is a single falsifiable assertion; ≥ 1 evidence reference; method stated.
- **recommendation** — all three impact declarations (business, user, dopamine) with metric/baseline/target/window; rollback with watch signals; dopamine metric NOT on the prohibited list.
- **metric** — definition unambiguous enough to reimplement; unit and direction-of-good declared.
- **incident_report** — all lessons marked `verified: true/false` with evidence; `root_cause.pattern_refs` set so propagation can match other platforms.

## 4. Relationships

Declare `relates-to` / `supersedes` / `contradicts` / `depends-on` / `derived-from` edges with graph node IDs. If you know your pack contradicts existing knowledge, declare it — undeclared contradictions lower your trust score when detected.

## 5. Provenance

Chain must be complete: every source (with URI + observation time) and every transformation (tool, version, parameters, actor). If you can't state where a number came from, don't publish it.

## 6. Confidence

State your asserted confidence (0.00–1.00) and the reasoning. Dot.Brain recomputes on ingestion; large deltas between asserted and computed confidence affect trust scores.

## 7. Sign & Publish

Canonicalize (RFC 8785), sign (Ed25519, platform key + contributor keys), publish to your tenant topic. Expect an ack or a validation report; handle `DKP_*` error codes per [../brain.dkp.md](../brain.dkp.md).

---

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-01 | DKP Architect (prompt 02) | Initial template |

## Open Questions

- None.
