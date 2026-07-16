# 18 — Screen-by-Screen Specification

| Field | Value |
|---|---|
| Purpose | Wireframe-ready specification of every MVP screen: data, actions, permissions, states, mobile behavior, audit, acceptance criteria |
| Audience | Product designers, frontend engineers, QA |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Design lead |
| Dependencies | 17 (IA), 12 (permissions), 14 (states), 15 (FRs), 19 (notifications) |
| Source references | Master prompt §12; D-03, D-06, D-08 |
| Assumptions | ASM-03, ASM-16 |
| Open questions | — |
| Approval required | Design + product leads |
| Last updated | 2026-07-16 |

## 1. Global state standards (apply to every screen; not repeated below)

- **Loading:** skeleton placeholders (no spinners >300ms without skeleton); long operations (uploads, analysis) show determinate progress where possible.
- **Empty:** every list/collection has a purposeful empty state: icon + one-line meaning + primary next action; never a bare "no data".
- **Error:** inline field errors on blur; page-level error banner with retry + support reference ID; destructive/irreversible actions always confirm with typed intent or explicit summary.
- **Warning:** amber banners for approaching expiry, stale data, unverified AI output; warnings never block, blockers always explain the unblocking path.
- **Mobile (≥360px):** single-column stacking; tables collapse to cards with key fields; uploads supported via camera/file picker; internal admin screens are desktop-optimized (functional but not polished on mobile in MVP).
- **Audit:** every mutating action on a screen emits the audit events listed in 14/36; screens listed below only note screen-specific extras.
- **Acceptance criteria:** each screen's ACs live in 72 under `AC-<screen-id>`; the notes below are the design contract.

Per-screen format: **User / Purpose / Data / Actions / Permissions / Notable states / Mobile / Audit extras**.

## 2. Public & onboarding

**SCR-P1 Landing.** User: anonymous. Purpose: explain capital readiness (what it is / is not — limitation statement visible), the process (5 steps), eligibility basics, fees disclosure link. Data: static + published band definitions. Actions: Apply (business), provider application, partner application, language toggle. Notable: no marketing claims from prohibited list (02 §3); "not a lender" statement above the fold. Mobile: full.

**SCR-P2 Business application.** User: R-PA. Purpose: preliminary application (FR-APP-01). Data: form sections (business identity, activity, financing need, contact). Actions: save-resume (magic link), submit, withdraw. Notable states: submitted-confirmation explains screening + SLA (5 business days, 51); duplicate-warning info panel. Audit: applicant.submitted. Mobile: full.

**SCR-P3 Capital-provider application.** User: prospective provider. Purpose: intake for manual onboarding (54). Data: institution identity, type (bank/fund/family office/…), jurisdictions, contact, mandate summary. Actions: submit. Notable: sets expectation of manual diligence + agreement.

**SCR-P4 Partner application.** Like P3 for legal/compliance/accounting partners (58).

**SCR-P5 Account creation.** User: invited user (any org). Purpose: activate account. Data: invite context (org, role — read-only), password/MFA enrollment. Actions: accept invite, enroll MFA (mandatory before sensitive data, 43). Notable: expired-invite state with re-request.

**SCR-P6 Identity verification.** User: R-BA (first login), signatories. Purpose: individual ID verification per overlay (manual upload MVP-1, vendor ASM-06 later). Data: ID document capture, selfie step placeholder (vendor phase). Notable: pending-review state; failure → support path.

**SCR-P7 Organization setup.** User: first R-BA. Purpose: confirm org legal identity, invite team (R-BM), notification prefs. Actions: invite/remove members, set roles.

**SCR-P8 Application status.** User: R-PA. Purpose: track screening (14 §2 states) + answer RFI-preliminary. Data: state, submitted date, SLA hint, RFI thread. Actions: respond to RFI, withdraw. Notable: rejection state shows reason category + reapply window; acceptance state links to account creation.

## 3. Borrower workspace

**SCR-B1 Dashboard.** User: R-BA/R-BM. Purpose: single source of "where am I and what's next". Data: lifecycle status header; checklist coverage % (explicitly labeled "completeness, not score"); open tasks (top 5 by due date); open clarifications; latest approved score chip (if any, ASM-16) + confidence badge; recent activity feed. Actions: continue-next-step CTA (deep link to first incomplete requirement). Empty (fresh workspace): guided 4-step setup wizard entry. Mobile: full.

**SCR-B2 Company profile.** Purpose: FR-WS-01 capture. Data: overlay-driven identity fields (CR: cédula jurídica etc.), sections with completeness meters. Actions: edit sections; submit-section. Warning state: post-approval edits show material-change notice (FR-SCORE-08). Audit: profile.updated w/ field diff.

**SCR-B3 Report a change.** Purpose: structured material-change disclosure (duty from attestation). Data: change type taxonomy (ownership, debt, litigation, revenue event, other), description, docs. Actions: submit → creates reviewer task + possible reassessment. Audit: change.reported.

**SCR-B4 Ownership & management.** Purpose: FR-WS-02. Data: cap table editor (person/entity rows, %), UBO chain visualization (tree), directors/officers, related parties. Actions: add/edit rows, upload supporting IDs (P4-classified). Validations visible inline (sum-to-100, UBO coverage). Warning: unresolved UBO coverage shows GATE-02 implication plainly.

**SCR-B5 Capital request.** Purpose: FR-WS-03. Data: amount/currency/purpose/module, use-of-proceeds line editor (sum check), repayment source builder (structured: source type + narrative), collateral offered, tenor, timing. Actions: save, submit-for-checklist; post-publication edits locked behind amendment request. Warning: missing repayment source shows CAP-02 consequence ("assessment cannot exceed Developing band").

**SCR-B6 Personalized checklist.** Purpose: evidence requirements hub (FR-WS-04). Data: grouped by dimension; each item: evidence type, why-it-matters (linked control, plain language), status chip (requested/uploaded/processing/verified/rejected/expired), period + max-age, format guidance. Actions: upload against item, mark-not-applicable (only where item is `conditional`, requires reason → reviewer confirms per 24 §4), view rejection reason. Filters: status, dimension. Empty: profile-incomplete explainer. Mobile: cards.

**SCR-B7 Document room & upload.** Purpose: FR-DOC-01/02/03/05. Data: folder tree by taxonomy; file cards (name, type, version, state, period, expiry); upload flow: drag-drop → scan progress → AI feedback panel (recognized type, detected period, issues, next steps — labeled AI-assisted, pending human verification). Actions: upload new version, download own docs, view version history, respond to defects. Error: malware-quarantine state with guidance. Audit: document.viewed/downloaded (all roles).

**SCR-B8 Clarification center.** Purpose: FR-REV-02 borrower side. Data: open/answered threads; each: question, related document/control (plain-language), due date. Actions: answer (text + attach), request extension. Empty: "no open questions — you're up to date."

**SCR-B9 Task list.** Purpose: FR-WS-05. Data: tasks w/ source (checklist/clarification/RFI/manual), assignee, due, status. Actions: assign to member (R-BA), complete (only when underlying object satisfied — UI explains).

**SCR-B10 Readiness score.** Purpose: D-06 results home. Data: score (large), band chip + band definition, evidence-confidence badge + explanation, limitation statement (fixed component, non-dismissable), dimension bar chart (8 bars w/ earned/possible), strengths (top 3), critical blockers (gates/S1-S2 flags in borrower-visible form), conditions, delta panel vs previous approved assessment, assessment validity/expiry date, External Risk Context panel (level + factors + mitigants; visually separate, labeled "not part of your score"). Actions: view dimension detail, view action plan, download report (SCR-B15), request reassessment, start attestation/consent flow (if eligible). States: no-assessment-yet (progress explainer); in-review (status + SLA); expired (renewal CTA). Audit: report.viewed. Mobile: full.

**SCR-B11 Dimension details.** Purpose: drill-down. Data: per dimension: score, controls list (name, plain-language description, maturity 0–4 with anchor text, evidence links, N/A + reason), what-would-improve hints (from 28). Not shown to borrower: internal reviewer notes, restricted flags.

**SCR-B12 Flags & conditions.** Purpose: transparency on issues (23 §6 borrower view). Data: visible flags: title, severity in plain language, what it blocks/caps, required resolution evidence, status; conditions attached to approval. Actions: submit resolution evidence (routes to reviewer). Restricted flags excluded entirely (no placeholder hint).

**SCR-B13 Gap-remediation plan.** Purpose: 28 output. Data: prioritized actions: what, why (control link), expected effect (range language, never promise), effort estimate, templates/partner referral links, status. Actions: mark in-progress/done (evidence-linked), request partner referral. Delta simulation explicitly labeled "estimate".

**SCR-B14 Assessment history.** Data: timeline of approved assessments: date, model version, score/band/confidence, key changes; comparison view (two assessments side-by-side by dimension).

**SCR-B15 Readiness report.** Purpose: view/download the borrower artifact (27 §3). Data: rendered report + PDF download. Watermarked "prepared for <org>".

**SCR-B16 Capital-provider activity.** Purpose: consent transparency (D-08). Data: which provider orgs have access (grant dates/expiry), RFIs received/answered, EOIs received (with accept/decline actions), consent records + revoke action (confirm w/ consequences). Audit: consent.revoked.

## 4. Internal administration

**SCR-A0 Ops home.** All internal roles. My queues w/ counts + SLA breaches highlighted; assignments; announcements.

**SCR-A1 Application queue.** R-AR. Table: applicant, submitted date, sector, amount, status, age, assignee. Actions: claim, assign. Filters/sort. SLA badge >3d amber, >5d red (51).

**SCR-A2 Applicant review.** R-AR (+R-SA escalation). Data: full form, duplicate matches panel, screening checklist (interactive, each item evidence/notes), manual screening log (sanctions/adverse-media protocol results), decision panel. Actions: accept/reject/waitlist/RFI + reason codes; escalate. Audit: applicant.* + screening.completed.

**SCR-A3 Organization management.** R-IA. Orgs table (type, status, members, created); detail: members/roles/invites, suspension w/ reason, conflict declarations, break-glass initiation (dual-approval flow), consent ledger view.

**SCR-A4 Assessment queue.** Internal. Assessments by state (14 §3), SLA, assigned reviewers per dimension group, blockers summary.

**SCR-A5 Document review queue.** R-FR/R-LR/R-CR by class. Data: doc preview (page viewer w/ extraction overlays: bounding boxes on extracted fields), schema fields with confidence + accept/correct controls, classification confirm/change, defect actions (reject w/ taxonomy), reconciliation panel (related docs + matched/mismatched fields). Actions: verify, reject, raise clarification, raise flag. Audit: document.verified/rejected + field.corrected.

**SCR-A6 AI exception queue.** MVP-2. Data: exceptions (contradiction, low-confidence, staleness, schema-fail) w/ evidence side-by-side, AI-drafted explanation (labeled), resolution options (accept version A/B, request clarification, create flag, manual value w/ citation). Never an "average values" option (CRF App B).

**SCR-A7 Control assessment.** Dimension reviewers + R-SA summary mode. Data: control grid per dimension: control, weight, AI-suggested maturity (MVP-2, labeled w/ rationale), reviewer maturity selector (0–4 anchors shown), evidence citations (required), N/A per applicability profile (locked where profile disallows), notes (internal). Live provisional score panel (internal-only watermark). Section sign-off per reviewer. Audit: control.assessed, section.signed.

**SCR-A8 Gate & red-flag review.** R-CR primary. Data: gates GATE-01…07 status board w/ evidence; screening hits list (match details, disposition options: true/false/needs-info w/ mandatory rationale); flags register (severity, effect, resolver, visibility class incl. restricted toggle — restricted requires R-SA co-sign). Audit: gate.dispositioned, flag.created/resolved, screening.dispositioned.

**SCR-A9 Override approvals.** R-SA. Data: pending overrides: proposer, control/score effect, reason code, evidence, expiry, system-vs-proposed values, band impact (ASM-09 limit shown). Actions: approve/reject w/ note. Audit: override.approved/rejected.

**SCR-A10 Opportunity packaging.** R-IA. Data: package composer: summary fields, disclosure set (flags/conditions visibility per 23 §6 with lint results), document-room selection (approved subset, P4 redaction status), provider selection + consent verification status, package preview (exact provider view). Actions: run disclosure lint, submit for R-SA approval. Blocked state: lint failures enumerate exact violations.

**SCR-A11 Provider-access management.** R-IA. Grants table (provider, opportunity, granted, expiry, activity), extend/revoke, RFI routing, EOI log. Audit: access.granted/extended/revoked.

**SCR-A12 Audit log viewer.** R-IA/R-SA/R-CR(scope)/R-AUD. Filter by actor/object/event/time; event detail w/ hash-chain verification indicator; export request flow (dual-approval, FR-AUD-01).

**SCR-A13 Scoring-model configuration.** R-IA draft, R-SA publish. Data: model versions list; draft editor: controls/weights/applicability/caps/bands/thresholds w/ validation (weights sum, band continuity); diff vs current; impact simulation on historical assessments (read-only what-if); changelog. Publish = dual-control. Audit: scoring_model.drafted/published.

**SCR-A14 Jurisdiction configuration.** Same pattern for overlays (63 schema): evidence sets, identity fields, entity types, disclaimers, consent texts (versioned, counsel-source noted), retention values.

## 5. Capital-provider workspace

**SCR-C1 Provider dashboard.** R-CPA/R-CPN. Data: authorized opportunities (cards: sector, amount, band chip, confidence badge, status, access expiry), open RFIs, EOI statuses. Empty: "no opportunities shared with your institution yet" + mandate contact. Mobile: cards.

**SCR-C2 Opportunity list.** Table/cards w/ filters (sector, amount, band, status). Saved filter sets. No cross-provider info ever.

**SCR-C3 Opportunity profile.** Purpose: standardized summary (D-08). Data: business profile (disclosed subset), capital request, readiness score + band + confidence + limitation statement, dimension bars, key risks & conditions (disclosed set), External Risk Context panel, assessment validity, package download (watermarked PDF per 27 §5). Actions: open room, RFI, EOI, save, note. Audit: opportunity.viewed, package.downloaded.

**SCR-C4 Readiness summary tab / C4b Document room tab.** C4: dimension drill-down (control-level summaries; evidence-confidence per dimension; no internal notes). C4b: approved docs (viewer w/ watermark overlay, download tracked), doc metadata (type, period, verified date). No upload.

**SCR-C5 RFI flow.** Compose (structured category + free text, optional doc reference), thread view, status. R-CPN drafts, R-CPA submits (12).

**SCR-C6 EOI flow.** R-CPA only. Form: indicative amount/structure/tenor/timeline/conditions + non-binding acknowledgment checkbox (mandatory). Status: submitted → borrower accepted/declined → introduction (contact exchange per consent). Audit: opportunity.eoi_submitted.

**SCR-C7 Team notes.** Provider-internal notes on an opportunity; visible only within provider org (12 §2). Labeled as such.

**SCR-C8 Saved opportunities.** Bookmarks + notes summary.

**SCR-C9 Status tracking.** Provider's pipeline view of their opportunities (published → RFI → EOI → external underwriting → outcome) + prompt to report outcomes (feeds 37 metrics).

## 6. Screen-level acceptance criteria pattern (full set in 72)

For each screen: (1) renders all specified data for permitted roles, 403s others at API level; (2) all listed actions produce the mapped state transitions + audit events; (3) empty/loading/error/warning states match §1 standards; (4) limitation statement present wherever score/grade renders; (5) mobile layout functional at 360px; (6) ES/EN complete; (7) no prohibited terms (copy lint).
