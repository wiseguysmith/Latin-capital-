# 15 — Functional Requirements

| Field | Value |
|---|---|
| Purpose | Testable functional requirements for the MVP, keyed to screens, services, events, CRF controls, and phases |
| Audience | Engineering, QA, product |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | 14 (states), 12 (permissions), 18 (screens), 42 (APIs), 72 (acceptance criteria) |
| Source references | Master prompt §4, §8; CRF §14–16 |
| Assumptions | ASM-07, ASM-10, ASM-11, ASM-16 |
| Open questions | OQ-09 |
| Approval required | Product owner |
| Last updated | 2026-07-16 |

## 0. Conventions

ID scheme `FR-<AREA>-##`. Classification: **MVP-1/2/3** (phase) or **POST** / **FUTURE**. Every FR's full acceptance criteria live in 72 under the same ID; audit events referenced by name are defined in 36; screens in 18; services in 41/42. Requirements below state actor, trigger, behavior (main flow), key alternates/exceptions, business rules, data, audit, notifications, and permissions in compact form. "The system shall" is implied.

## 1. Intake & applications (FR-APP)

**FR-APP-01 Preliminary application capture — MVP-1.**
Actor R-PA; trigger "Apply" on SCR-P1. Multi-step form (SCR-P2) collecting: legal name, country (CR only at launch, overlay-driven list), sector (taxonomy in 24 §5), years operating, revenue band, requested amount + currency, capital purpose (CRF §10 list), existing debt (y/n + amount band), contact person + verified email, referral source. Save-and-resume via magic link. Preconditions: processing-consent checked (stores consent record ENT-24). Exceptions: unverified email blocks submit; prohibited-activity self-declaration shows warning + still allows submit (flagged for screening). Business rules: one active application per legal-entity name+country (duplicate soft-match warning to R-AR, not hard block). Data: ENT-01 Applicant, ENT-24 Consent. Audit: applicant.draft_created/submitted. Notifications: NOTIF-APP-01 ack. Permissions: R-PA own-record only.

**FR-APP-02 Screening queue & decisioning — MVP-1.**
Actor R-AR; SCR-A1/A2. Queue sorted by age; filters (status, sector, amount). Detail view: form data, duplicate matches, screening checklist (51 §5: completeness, prohibited activities OQ-10 list, manual sanctions/adverse-media protocol, plausibility). Decisions: accept / reject(reason code) / waitlist(reason) / request-info(structured questions). Accept requires checklist complete; out-of-range amount or excluded-sector acceptance escalates to R-SA (14 §2). Data: ENT-02 ScreeningRecord. Audit: applicant.* transitions. Notifications: NOTIF-APP-02…05.

**FR-APP-03 Acceptance provisioning — MVP-1.**
Trigger applicant.accepted. Auto-create: borrower org, R-BA invitation, workspace shell, assessment record in `workspace_setup`, jurisdiction overlay binding (CR), checklist pending profile completion. Idempotent on retry. Audit: org.created, assessment.workspace_activated.

**FR-APP-04 Waitlist management — MVP-1.** R-AR promotes/demotes waitlisted applicants; waitlist entries auto-expire after 120 days with notification and reapply path.

## 2. Workspace & profile (FR-WS)

**FR-WS-01 Company profile — MVP-1.** R-BA/R-BM complete structured profile (SCR-B2): legal identity (overlay-driven fields: razón social, cédula jurídica for CR), addresses, sector, employees, business description, key customers/suppliers (concentration inputs for B5), org chart upload. Field-level completeness tracked; edits after assessment approval trigger material-change evaluation (FR-SCORE-08).

**FR-WS-02 Ownership & management capture — MVP-1.** Structured cap table: shareholders (entity/person), percentages (must total 100% ±0.5 with explanation), UBO chain to natural persons with ID data (P4 class), directors/officers, related-party register. Validations: cycle detection in ownership graph; UBO <25% aggregate coverage triggers GATE-02 enhanced review flag (threshold overlay-configurable). Data: ENT-05/06/07. Related CRF controls: E1, E2, C4, F1.

**FR-WS-03 Capital request — MVP-1.** Amount, currency (USD/CRC, ASM-04), purpose (module selector, 25), use-of-proceeds line items (must sum to amount ±1%), preferred structure, tenor, proposed repayment source (required free+structured field — feeds CAP-02 check), collateral offered (type taxonomy, 26), timing. Editing after publication requires R-SA-approved amendment (versioned). Controls: G1, G2, G3.

**FR-WS-04 Personalized checklist generation — MVP-1.** On profile+request completion: deterministic rules engine (41) composes checklist = universal core file (26 §2) + business-type set + financing-module set + jurisdiction overlay set + amount-band adjustments. Each item: evidence type (EV-xxx), required/conditional/optional, period, max age, format guidance, linked controls. Regenerates on request changes with diff shown; manually added reviewer items supported. Controls: all. Data: ENT-09 ChecklistItem.

**FR-WS-05 Tasks & deadlines — MVP-1.** Tasks auto-created from checklist items, clarifications, RFIs; manual tasks by reviewers; assignee (R-BA can reassign to R-BM), due dates, overdue notifications (19), completion tied to underlying object state (a document task completes only when the document reaches `verified`).

## 3. Documents (FR-DOC)

**FR-DOC-01 Secure upload — MVP-1.** Formats/sizes per ASM-11; client-side chunked upload; server-side malware scan before availability (quarantine + notify on fail); SHA-256 hash + immutable original stored (CRF §16 layer 1); metadata: uploader, org, timestamp, declared type (optional), source channel. Audit: document.uploaded.

**FR-DOC-02 Versioning & supersede — MVP-1.** New upload against same checklist item creates version n+1; prior versions retained read-only; `superseded` state per 14 §4; version history visible to borrower (own docs) and reviewers.

**FR-DOC-03 Per-document AI feedback — MVP-2.** After upload: pipeline per 30 §4 (classify → extract per 32 schema → period/parties/amounts/signatures → control mapping → defect detection → provisional confidence). Borrower-visible outcome within 10 min (NFR-04): recognized type, period detected, issues found (missing pages, illegible, wrong period, unsigned), and next steps. Material-field or low-confidence results enter verification queue rather than auto-accept (thresholds 31 §5). Audit: document.classified, document.extraction_completed.

**FR-DOC-04 Manual review & verification — MVP-1.** Reviewer verifies/rejects with defect taxonomy (26 §5); verified fields become authoritative with provenance (verifier, timestamp, source region). Un-verify only by R-SA with reason.

**FR-DOC-05 Document room organization — MVP-1.** Folder structure auto-derived from evidence taxonomy (26); search/filter by type, state, period, control; bulk download for internal roles only (audited); borrower export of own room.

**FR-DOC-06 Expiry & freshness — MVP-2.** Nightly job flags documents past max-age (26 per-type) → `expired`, checklist item reopens, borrower notified; approaching-expiry warnings at T-14d.

## 4. AI analysis (FR-AI) — governance boundaries in 31 control these

**FR-AI-01 Classification service — MVP-2.** ≥ config threshold (default 0.85) auto-accept; below → human classification task. Misclassification correction is training feedback (logged, 31 §8).
**FR-AI-02 Structured extraction — MVP-2.** Schema-validated output only (32); non-conforming output → retry policy (30 §8) → manual entry task. Field-level confidence + bounding-box provenance stored.
**FR-AI-03 Cross-document reconciliation — MVP-2.** Deterministic comparators over extracted fields (revenue vs bank inflows vs tax filings; debt schedule vs bureau/registry/liens; ownership consistency across documents). Material mismatch (materiality per control, 21) → exception ENT-15, never averaged (CRF App B). AI drafts explanation; human resolves.
**FR-AI-04 Contradiction & staleness detection — MVP-2.** Rules + AI-assisted detection; outputs to AI exception queue (SCR-A6).
**FR-AI-05 Clarification drafting — MVP-2.** AI drafts borrower questions from defects/exceptions; reviewer approves/edits before send (no auto-send to borrower). Audit: assessment.clarification_requested carries drafter=AI, approver=human.
**FR-AI-06 Explanation drafting — MVP-2.** AI drafts: borrower-facing dimension explanations, internal reviewer report, provider summary — all with citations to evidence fields; deterministic numbers injected from scoring engine, never generated (31 §4).
**FR-AI-07 Remediation recommendations — MVP-2.** Draft action plans per 28; reviewed before borrower release in MVP.

## 5. Scoring & assessment (FR-SCORE)

**FR-SCORE-01 Deterministic engine — MVP-1.** Implements 20 §4–6 exactly: maturity 0–4 per applicable control; contribution = weight × maturity/4 (decimal); dimension and total roll-ups; N/A reallocation per 24; caps per 23; band mapping; reproducibility: same inputs + model version → identical output (test TC-SCORE-01). No network calls to AI services from the engine.
**FR-SCORE-02 Maturity assessment capture — MVP-1.** Reviewers set maturity with mandatory evidence citations (≥1 evidence link or explicit "absence" note for level 0); AI-proposed maturities (MVP-2) appear as suggestions with rationale, never auto-committed.
**FR-SCORE-03 Evidence confidence computation — MVP-2 (manual grade entry MVP-1).** Per 22: document→control→dimension→assessment; grade A–D separate from score everywhere in UI/reports/API.
**FR-SCORE-04 Gates enforcement — MVP-1.** GATE-01…07 statuses block per 23 §3; no borrower score publication while any gate unresolved (publication = borrower result release, not internal computation).
**FR-SCORE-05 External risk context — MVP-1.** Reviewer-entered 1–5 level + factor list + mitigants (CR context library seeded, 60); displayed as separate panel; never arithmetically combined with score (test TC-SCORE-04).
**FR-SCORE-06 Versioned scoring model — MVP-1.** Model bundle (controls, weights, applicability, caps, bands, confidence thresholds) versioned semver; assessments pin the version used; historical results reproducible; model publish = R-IA draft + R-SA approve (12 §6) + changelog (20 §8).
**FR-SCORE-07 Overrides — MVP-1.** Per 23 §7: proposer ≠ approver; reason code, evidence, expiry (≤ assessment expiry, ASM-09), band-movement limit (ASM-09); system score retained alongside published score; override register reportable (37).
**FR-SCORE-08 Material change & reassessment — MVP-2.** Profile/request/document changes after approval evaluated against material-change rules (list in 20 §9) → assessment.reassessment_required; borrower duty-to-disclose supported by structured "report a change" flow (SCR-B3).
**FR-SCORE-09 Score visibility — MVP-1.** Borrower sees results only in `approved`/`approved_with_conditions`/`not_ready` states (ASM-16), always with limitation statement, band, confidence grade, dimension breakdown, strengths, gaps, blockers, conditions, actions, and delta vs previous approved assessment (D-06).
**FR-SCORE-10 Fee firewall — MVP-1.** No billing-related attribute exists in scoring inputs schema; attempt to add one fails schema validation + CI check (D-15).

## 6. Review workflow (FR-REV)

**FR-REV-01 Queues — MVP-1.** Document review, AI exceptions (MVP-2), control assessment, gate/flag review, final approval; SLA timers (50 §5), assignment, workload view (SCR-A4…A8).
**FR-REV-02 Clarification lifecycle — MVP-1.** Draft (AI or reviewer) → approve → send → borrower answer → resolve/reopen; linked to evidence + controls; unanswered ≥ config days escalates task.
**FR-REV-03 Flag management — MVP-1.** Create (manual MVP-1; system/AI-proposed MVP-2), severity S1–S4, effect binding (gate/cap/condition/warning), resolver role, evidence required, per-audience visibility incl. restricted class (23 §6).
**FR-REV-04 Final approval — MVP-1.** R-SA package view: score + confidence + gates + flags + overrides + reviewer sign-offs + External Risk Context; decisions approve / approve-with-conditions (enumerated) / return (to named queue with notes) / not-ready. Segregation enforced (12 §6).
**FR-REV-05 Internal reviewer report — MVP-2.** Generated artifact per 27 §4 for committee-style record.

## 7. Opportunities & providers (FR-OPP, FR-CP)

**FR-OPP-01 Packaging — MVP-3.** Compose provider package (27 §5): standardized summary, profile, request, score/band/confidence, dimension detail, disclosed risks/conditions, external context, approved document room selection; disclosure lint blocks restricted-flag or P4-unredacted leakage (23 §6).
**FR-OPP-02 Publication & grants — MVP-3.** Named-provider grants with expiry (ASM-12); borrower consent verification; R-SA approval; unpublish/revoke ≤1h propagation (NFR-09). Audit: opportunity.published, access.granted/revoked.
**FR-OPP-03 RFI — MVP-3.** Provider submits structured RFI → internal triage (route to borrower as task or answer from approved data) → response w/ optional new shared docs (R-BA action) → provider notified; full thread retained per party visibility rules.
**FR-OPP-04 EOI — MVP-3.** R-CPA-only submit; indicative non-binding fields (amount, structure, tenor, timeline, conditions); borrower accept/decline for introduction; explicit non-binding copy (02 §3).
**FR-CP-01 Provider workspace — MVP-3.** Dashboard (authorized opportunities only), opportunity profile, readiness summary, document room (view/download watermarked + tracked), saved opportunities, status tracking, internal provider notes (never visible outside provider org).
**FR-CP-02 Provider onboarding — MVP-3.** Manual diligence per 54; agreement acceptance recorded; R-CPA seat provisioning; mandate profile (sectors/amounts/structures) stored for curation (not auto-matching, D-18).

## 8. Administration, audit, notifications (FR-ADM, FR-AUD, FR-NOT)

**FR-ADM-01 Org & user admin — MVP-1.** CRUD per 12; suspension cascades (14 §6); conflict declarations; break-glass flow (12 §7).
**FR-ADM-02 Jurisdiction overlay config — MVP-1.** CR overlay v1 seeded (60); overlay entities per 63; draft/publish with R-SA approval; assessments pin overlay version.
**FR-ADM-03 Fee disclosure page — MVP-1.** Static disclosure of current fee schedule (ASM-10/D-15); no payment processing.
**FR-AUD-01 Audit log — MVP-1.** Per 36: append-only, hash-chained, queryable by object/actor/time; viewer for R-IA/R-SA/R-CR(compliance scope)/R-AUD; export with approval.
**FR-AUD-02 Access & download tracking — MVP-1/3.** Every document view/download recorded with user, org, IP, purpose-context; provider download applies watermark (ASM-12).
**FR-NOT-01 Notification engine — MVP-1.** Event-driven per 19 matrix; email + in-app; per-user digest preferences; critical notices (security, consent, expiry) non-suppressible.

## 9. Cross-cutting behaviors

- All user-facing errors follow 18 §1 error-state standards; all lists paginate; all timestamps stored UTC, displayed local (CR default GMT-6).
- ES/EN localization (ASM-03); legal texts only from counsel-validated strings (OQ-14).
- Any screen rendering score/confidence renders the limitation statement component (02 §3) — single shared component, test-enforced.
