---
title: Dot.Engage — Platform Knowledge
version: 1.0.0
status: active
owners: [Engage Platform Lead, Registry Agent]
platform-id: dot-engage
dkp-version: 1.0.0
integration-status: registered
last-review: 2026-08-02
---

# Dot.Engage

> **Platform-owned source:** [Dot.Engage's wiki.md](https://github.com/sakhilebhayi/Dot.Engage/blob/main/wiki.md) — the platform's own knowledge home. This document is Dot.Brain's ingested view; the wiki is authoritative for what the platform actually is.

## 1. Purpose & Business Domain

**Domain correction (2026-08-02):** despite the ecosystem registry's `campaign` icon suggesting a marketing/engagement-campaign product, the real, built codebase is a contract-sharing, real-time chat, and video-call document-signing platform — not CRM/proposals/scheduling as an earlier README claimed. Real entities: Contract, ContractSignature, ContractVersion, Conversation, Message, VideoSession, VideoSessionSignature. The icon/name mismatch is a registry-side issue, not a code issue — flagged for whoever owns `os/Appendix.md`'s icon mapping.

## 2. Entities Owned

| Entity | Graph node type | Natural key | Notes |
|---|---|---|---|
| Contract | `entity:asset` | contract ID | Team-scoped |
| ContractVersion | `observation` | contract × version | Versioned document content |
| ContractSignature | `outcome` | signature ID | Signing record |
| Conversation | `entity:process` | conversation ID | Real-time chat, team-scoped |
| Message | `observation` | message ID | Chat message |
| VideoSession | `entity:process` | session ID | Live video-call, team-scoped |
| VideoSessionSignature | `outcome` | session × signature | In-call document signing |

## 3. Events Emitted

Two dead events found during integration: `VideoSessionStarted` and `ContractShared` are declared and have listeners registered, but nothing in the codebase actually dispatches them — flagged as a real gap, not fixed (dispatch-point decision belongs to the platform team).

## 4. Knowledge Packs Published

None. No DKP manifest or publish pipeline exists.

## 5. Intelligence Consumed

None currently subscribed.

## 6. Cross-Platform Relationships

```mermaid
flowchart LR
    U[Team member] -->|shares/signs| DE[Dot.Engage: Contracts]
    U -->|chats, video calls| DE2[Dot.Engage: Conversations/VideoSessions]
```

No cross-platform data exchange with other Dot Ecosystem platforms yet beyond shared ecosystem SSO and the shared `infodot` database.

## 7. Tenancy Model

Team-scoped via `team_id` on Contract/Conversation/VideoSession. The integration pass found and fixed the most severe issue across all 7 newly-integrated platforms' dashboard code: a prior commit added `/dashboard` stat queries (`Contract::count()`, `Conversation::whereNotNull(...)->count()`, etc., plus actual `recentContracts`/`activeConversations` record listings) with **zero team scoping** — every team's dashboard showed every other team's aggregate counts and actual contract/conversation records. This was a live, exploitable cross-tenant data leak, not a theoretical one. Fixed by scoping every query to `Auth::user()->currentTeam->id`. Also closed a missing-authorization gap in three Livewire components (`VersionHistory`, `InCallDocumentViewer`, `ParticipantList`) that performed by-ID lookups with no Policy check.

## 8. Dopamine Surface

Explicitly checked given the platform's name and original "engagement" framing — clean. No streak/badge/points/leaderboard/dark-pattern mechanics found anywhere in the codebase; the notification/unread-count components are plain inbox-style counters, not attention-capture loops. Consistent with the manifesto's ethical-engagement principle.

## 9. Active Recommendations

None — no Knowledge Pack publishing yet.

## 10. Incident History Summary

**Real, live cross-tenant data leak** (§7) — the most severe finding across this 7-platform integration batch. Closed 2026-08-02, same day it was found, before any known exploitation. Introduced by an incomplete prior "fix" commit (`5dae85f`) that added the dashboard queries without team scoping.

## Verified Infrastructure State (2026-08-07)

Confirmed directly against the real repo during the ecosystem-wide standardization + code-quality pass (full 26-platform summary: [brain.platforms.md](../brain.platforms.md) change log, v1.0.21):

- **Legal/branding/auth** — branded Markdown-mail theme, complete POPIA-aligned Privacy Policy/Terms/Cookie Policy naming **BluePin Inc**, guest auth pages restyled to match the welcome-page hero.
- **Laravel Boost** — `laravel/boost` ^2.5 installed; `.mcp.json`/`boost.json`/`CLAUDE.md` guideline block in place.
- **Code-quality pass** — Pint: 69 files reformatted, formatting-only. `composer audit`: this platform had the largest blast radius found in the pass — **25 advisories across 10 packages**, all patched: `laravel/framework` (temporary signed-URL path confusion, CRLF injection in the default email validation rule — CVE-2026-48019), `dompdf/dompdf` → 3.1.6 (6 issues: SVG file-existence leak, resource-exhaustion DoS ×2, local file read, chroot bypass), `spatie/laravel-medialibrary` → 11.23.0 (file-upload restriction bypass CVE-2026-48557, SSRF CVE-2026-48555), `symfony/http-foundation`/`http-kernel`/`mailer`/`mime`/`routing` (transitive, CRLF/SMTP injection, HEAD-request auth bypass, route-requirement bypass), `league/commonmark` (baseline DoS set). `npm audit`: patched Vite `server.fs.deny` bypass and `launch-editor` NTLMv2 hash disclosure. Full suite reconfirmed green (92 passed / 145 assertions) after every change.

## Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0.0 | 2026-08-02 | Repository Steward Agent | Initial registration. Platform audited: real domain corrected (contract/chat/video-signing, not CRM), a live cross-tenant dashboard data leak fixed, three Livewire IDOR gaps closed, favicon added (was completely missing), README corrected. |

## Open Questions

| Question | Owner → Approver |
|---|---|
| `os/Appendix.md`'s `campaign` icon for this platform no longer matches its real domain — should it be updated to something contract/signing-related? | Registry Agent → Chief Knowledge Engineer |
| `VideoSessionStarted`/`ContractShared` events are declared but never dispatched — should they be wired now, or is this intentionally deferred? | Engage Platform Lead → Architecture Agent |
| `routes/channels.php` broadcast-authorization callbacks were not audited this pass. | Engage Platform Lead → Security Agent |
