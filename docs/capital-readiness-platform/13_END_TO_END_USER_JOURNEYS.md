# 13 — End-to-End User Journeys

| Field | Value |
|---|---|
| Purpose | Narrative journeys tying screens, states, actors, and outputs together; the connective tissue between 14 (states), 15 (requirements), and 18 (screens) |
| Audience | Product, design, operations |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | 14, 18, 26, 27, 28 |
| Source references | CRF §14 (borrower journey), §15 (lender journey); D-02…D-08 |
| Assumptions | ASM-16 (score visibility after approval) |
| Open questions | OQ-09 |
| Approval required | Product owner |
| Last updated | 2026-07-16 |

## J1 — Borrower: from discovery to approved readiness assessment

1. **Discover & pre-screen** (SCR-P1): business owner reads what readiness is/is not (limitation statement visible pre-signup), sees eligibility basics (business-purpose, countries, amount range, excluded activities).
2. **Preliminary application** (SCR-P2): identity of business + contact person, country, sector, years operating, revenue band, requested amount/purpose, existing debt yes/no, how they heard. ≤15 minutes; save-and-resume. Consent to processing + screening (GATE-05 basis) collected here.
3. **Screening** (internal J4): applicant sees status (SCR-P8): Submitted → Under review → (RFI: answer extra questions) → Accepted / Waitlisted / Rejected. Rejection shows reason category + reapply guidance; acceptance explicitly states it is not a funding approval (D-02).
4. **Onboarding** (SCR-P5/P6): R-BA account creation, MFA, org setup, invite R-BM teammates.
5. **Workspace setup** (SCR-B1): guided sequence — company profile → ownership/management (UBO chart data) → capital request + use of proceeds → financing preferences. Completing these generates the **personalized checklist** (business type × financing type × CR overlay; 24/26).
6. **Evidence collection loop** (SCR-B6/B7): upload → per-document AI analysis returns feedback (type recognized, period, defects, missing pages) → clarification requests appear in Clarification Center (SCR-B8) → tasks/deadlines track progress. Progress bar = checklist coverage, not score.
7. **Full-package analysis** (system) at substantial completeness (D-04): reconciliation, contradictions, staleness; internal verification and scoring (J5). Borrower sees status "Assessment in progress — under human review".
8. **Results** (SCR-B10/B11/B12): after senior approval, borrower sees score, band, evidence grade, dimension detail, strengths, missing evidence, blockers, conditions, actions, delta vs prior — with the limitation statement (D-06).
9. **Remediation sprint** (SCR-B13): prioritized action plan (28) with expected impact ranges ("completing bank reconciliation typically affects dimension A by up to X points" — never a promise), templates, partner referrals. Borrower fixes gaps → requests reassessment.
10. **Attestation & consent to share** (SCR-B10 → publication flow): R-BA signs attestation; chooses/consents to named providers proposed by platform (D-08, manual curation D-18).
11. **Provider interaction**: RFIs arrive as tasks; borrower responds via workspace; sees provider activity summary (SCR-B16) — which orgs accessed, RFIs, EOIs. Negotiation/closing happen **off-platform** (boundary 02); borrower may update status ("in external underwriting", "funded externally").

## J2 — Capital provider: onboarding to EOI

1. **Application** (SCR-P3) → manual diligence + agreement incl. independent-underwriting attestation (54) → provider org + R-CPA account created.
2. **Access grant**: platform publishes specific opportunities to the provider (55). Provider dashboard (SCR-C1) lists only authorized opportunities (D-08).
3. **Review** (SCR-C2/C3/C4): standardized summary, readiness score + band + evidence grade + dimension detail, key risks/conditions, external risk context, approved document room (watermarked, tracked).
4. **Diligence dialogue**: RFIs (SCR-C5); answers + any new shared docs appear in-room.
5. **EOI** (SCR-C6): non-binding interest with indicative parameters (amount, structure type, timeline) — copy explicitly non-binding; borrower consent governs contact exchange.
6. **Downstream**: provider underwrites independently off-platform; reports outcome status (declined-external / funded-external + optional reason taxonomy) which feeds opportunity lifecycle (14 §5) and pilot metrics (37).

## J3 — Partner (legal / compliance / accounting)

Invited by R-IA to a specific workspace section → deal-scoped access → performs review → uploads report/opinion (becomes E3-tier evidence, 22) → findings appear in reviewer queues. No cross-workspace browsing.

## J4 — Internal: application screening (SOP 51)

Queue (SCR-A1) → R-AR opens applicant (SCR-A2): completeness check, prohibited-activity screen, preliminary sanctions/adverse-media check (manual protocol), duplicate check → decision: accept (creates borrower org + workspace shell), reject (reason code), waitlist, or RFI-preliminary → audit events for each (36).

## J5 — Internal: assessment production (SOPs 52, 53)

1. Document-review queue (SCR-A5): verify AI classification/extractions (P2) or classify manually (P1); resolve defects; approve documents → evidence records link to controls.
2. AI exception queue (SCR-A6): contradictions, low-confidence fields, staleness hits.
3. Control assessment (SCR-A7): dimension-owner reviewers (12 §2) set/confirm maturities with evidence citations; system computes provisional score continuously (internal-only).
4. Gate & flag review (SCR-A8): R-CR dispositions screening; flags assigned severity/effect (23).
5. Final review (SCR-A7 summary → R-SA): senior approver sees full package: score, confidence, gates, flags, overrides, reviewer trail → approve / approve-with-conditions / return / not-ready (14 §3). Approval releases borrower report (J1 step 8).

## J6 — Internal: publication (SOP 55)

Packaging (SCR-A10): select disclosure set (which flags/conditions visible per 23 §6), generate provider package (27 §5) → R-SA approval + verify borrower consent → grant provider access with expiry (ASM-12) → monitor RFI/EOI activity (SCR-A11).

## J7 — Auditor

Scoped grant (R-AUD) → read-only review of assessments, evidence, overrides, audit log → findings delivered off-platform; access logged and time-boxed.

## Journey-level guardrails

- No journey step ever displays approval/funding language (02 §3).
- Every AI-generated artifact shown to any user is labeled as AI-assisted and carries citations (31).
- All waiting states show expected SLA and current stage — no silent black holes (19).
