# Security Audit — Cyber Range Simulator (Hardened)

**Scope:** `cyber-range-simulator-hardened.html` (single-page local exercise tool)  
**Audience:** Local / offline defensive tabletop use  
**Verdict:** No blocking issues. Ready for production **local** use.

## Summary

| Area | Result |
|---|---|
| Network egress | Pass — CSP `connect-src 'none'`, no remote URLs |
| Supply chain | Pass — no npm/CDN/fonts; single self-contained file |
| XSS / HTML injection | Pass — `escapeHtml` on rendered dynamic strings |
| Persistent state | Pass — allowlisted keys, clamped strings, sanitized state |
| Privilege / live systems | Pass — no sockets, scans, credentials, or remote APIs |
| Content safety | Pass — defensive decision training only; no exploit steps |

## Threat model

Assumptions:

- Operator opens the file from disk or a trusted static host.
- Browser may allow `localStorage` for the origin / `file://` context.
- Facilitator and participants may paste untrusted text into notes fields.

Out of scope:

- Host OS compromise, malicious browser extensions, or shared-machine shoulder-surfing.
- Serving this file behind an authenticated multi-tenant web app (would need server headers and a different review).

## Controls implemented

### 1. Network isolation

- CSP: `default-src 'none'`; `connect-src 'none'`; `font-src 'none'`; `object-src 'none'`; `worker-src 'none'`; `frame-ancestors 'none'`; `base-uri 'none'`; `form-action 'none'`.
- Inline style/script only (`'unsafe-inline'`) because this is a single-file offline artifact without a nonce-issuing server.
- `referrer` meta set to `no-referrer`.
- No `<img src=http...>`, no remote fonts, no analytics beacons.

### 2. XSS hardening

- `escapeHtml()` applied to scenario text and control labels before `innerHTML` insertion.
- Report panel uses `textContent`, not `innerHTML`.
- Status messages use `textContent`.
- Script wrapped in an IIFE with `"use strict"`.

### 3. Storage hardening

- Storage key versioned: `cyberRangeSimulatorState.v1`.
- Scenario / difficulty values allowlisted.
- Text fields length-capped (200 / 4000).
- `sanitizeState()` validates phase indices, choice indices, scores, and control IDs.
- Legacy key cleared on save; load still accepts legacy key for migration.
- Save/load failures fail closed with a user-visible status message.

### 4. Content boundary

- Authorized-use notice in the UI.
- Scenarios describe fictional injects and defensive choices only.
- Report includes an explicit use-boundary statement.
- No payloads, shell commands, exploit PoCs, or live targeting guidance.

## Residual risks (accepted for local use)

1. **`'unsafe-inline'` in CSP** — required for a nonce-free single HTML file. Mitigated by no remote content and escaped dynamic HTML.
2. **`file://` quirks** — some browsers restrict or partition `localStorage` for local files. Save/load may be unavailable; exercise still works in-session.
3. **Shared browser profiles** — local saves are readable by other users of the same browser profile. Do not store sensitive real-incident data in notes.
4. **Print / PDF** — printed reports may include facilitator notes; treat as internal training records.

## Test checklist

- [x] Page loads with no console network requests (CSP / offline).
- [x] All four scenarios start and complete decision flow.
- [x] Control checkboxes update scorecard.
- [x] Report generation and print path function.
- [x] Save → reload → Load restores allowlisted fields.
- [x] Malformed `localStorage` JSON does not break the page.
- [x] Notes containing `<script>` render as text in the report (`textContent`).

## Sign-off

Audit complete. No blocking issues. Ready for production (local use).

Recommended repository description:

> Offline defensive tabletop exercise tool — 0 network requests, fully local
