# 19 — Notifications, Tasks, and Communications

| Field | Value |
|---|---|
| Purpose | Notification matrix, task model, and communication rules (channels, digests, escalations, language) |
| Audience | Product, engineering, operations |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | 14 (events), 36 (event names), 18 (screens) |
| Source references | D-03; master prompt §12 |
| Assumptions | ASM-03 (locale) |
| Open questions | — |
| Approval required | Product owner |
| Last updated | 2026-07-16 |

## 1. Channels & rules

- **In-app** (always) + **email** (per matrix). SMS/WhatsApp: FUTURE (evaluate for CR pilot if email engagement is poor — OQ candidate, not blocking).
- Locale: user preference; legal/consent notices always sent in counsel-validated locale strings (OQ-14).
- **Digest control:** users may switch non-critical notifications to daily digest. **Non-suppressible:** security events, consent changes, access grants/revocations, expiry of assessment/documents, RFI/EOI receipt.
- No sensitive content in email bodies (no scores, no document contents, no flag details) — emails are "you have X waiting" + deep link (44 §3).
- All sends logged (notification.sent) with template version.

## 2. Task model

Task = {source (checklist item | clarification | RFI | flag-resolution | manual | system), workspace/opportunity ref, assignee (user or role-queue), due date, priority, state (open/in-progress/blocked/done/cancelled), completion rule (object-bound where applicable — FR-WS-05)}. Escalation: overdue T+3d → org admin (borrower side) or queue lead (internal); T+7d → R-IA dashboard flag. SLA definitions per queue in 50 §5.

## 3. Notification matrix (MVP; template IDs = NOTIF-*)

| Event (36) | Borrower (R-BA/R-BM) | Internal | Provider | Notes |
|---|---|---|---|---|
| applicant.submitted | ack (APP-01) | queue badge | — | SLA statement included |
| applicant.info_requested | APP-02 email+app | — | — | |
| applicant.accepted / rejected / waitlisted | APP-03/04/05 | — | — | acceptance includes "not a funding approval" sentence (D-02) |
| assessment.workspace_activated | WS-01 welcome/setup guide | — | — | |
| document.requested (checklist/clarif.) | DOC-01 (digestible) | — | — | |
| document.verified / rejected / expired | DOC-02/03/04 | — | — | rejection includes defect + fix guidance |
| document.quarantined (malware) | SEC-01 (critical) | security alert | — | |
| assessment.clarification_requested | CLAR-01 | — | — | |
| assessment.review_required | — | queue + assignment | — | |
| assessment.approved / approved_with_conditions / marked_not_ready | RES-01/02/03 (critical) | — | — | links to SCR-B10; never includes score value in email |
| assessment.expired / reassessment_required | RES-04/05 (critical) | task | paused notice if published | |
| override.proposed / approved | — | R-SA queue / proposer | — | |
| opportunity.publication_approved | consent confirmation (PUB-01, critical) | — | — | |
| opportunity.published | — | — | PROV-01 new opportunity | |
| opportunity.rfi_submitted / rfi_answered | RFI-01 task | routing | RFI-02 | |
| opportunity.eoi_submitted / eoi_response | EOI-01 (critical) | log | EOI-02 | non-binding language in all EOI templates |
| access.granted / revoked / expiring(T-7d) | B16 update | — | ACC-01/02/03 | |
| consent.revoked | ack | task: verify revocation propagation | access-removed notice | |
| task.overdue escalations | per §2 | per §2 | — | |
| security (new device, MFA change, break-glass) | SEC-02 (critical) | SEC-03 | SEC-02 | |

## 4. Communication boundaries

1. All borrower↔provider communication flows through RFI/EOI structures — no free-form direct messaging in MVP (keeps consent + audit integrity). Introduction (contact exchange) happens only after borrower EOI acceptance (SCR-B16/C6).
2. Internal notes never leave internal surfaces; provider notes never leave the provider org (12 §2).
3. Templates: versioned, reviewed for prohibited language (02 §3) at CI (73 §6); legal-sensitive templates (consent, acceptance, rejection, EOI) require counsel-validated copy per locale.
4. Support channel (R-SUP) handles how-to questions; anything requesting legal/credit interpretation is routed to standard disclaimer + human escalation path — support never advises on improving scores beyond published remediation guidance (conflict rule, D-15).
