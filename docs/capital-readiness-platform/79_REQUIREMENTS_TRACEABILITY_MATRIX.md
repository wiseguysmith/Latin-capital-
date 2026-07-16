# 79 — Requirements Traceability Matrix

| Field | Value |
|---|---|
| Purpose | Mandatory matrix linking CRF principles/controls → requirements → stories → screens → entities → services → audit events → tests → owners → phases |
| Audience | QA, product, framework owner, auditors |
| Status | Draft v1.0 — maintained as the single coverage authority; updated with every FR/US change |
| Version | 1.0.0 |
| Owner | QA lead (maintenance) + Framework owner (CRF linkage) |
| Dependencies | all |
| Source references | Master prompt §18 |
| Approval required | QA + framework owner |
| Last updated | 2026-07-16 |

## 1. How to read

Rows trace **CRF principles and mechanisms** (the methodology's load-bearing rules) through the product. Column key: Req (15), Story (70), Screen (18), Entity (33), Service/Module (41), Event (36), Test (73/74 + 72 AC), Owner (operational role), Phase (75). A second table traces the 48 controls as a block (they share machinery). This matrix answers: *"exactly how is each important CRF requirement represented in the product?"* (master prompt §18).

## 2. Principle/mechanism traceability

| CRF source | Principle / rule | Req | Story | Screen | Entity | Service | Audit event | Test | Owner | Phase |
|---|---|---|---|---|---|---|---|---|---|---|
| §1, §21 | Readiness ≠ creditworthiness; limitation everywhere | AC-G4, FR-SCORE-09 | US-611 | B10, C3, B15 | ENT-21/36 | assessment | report.viewed | copy lint; AC-G4 | Compliance | 1 |
| §3 (6 layers) | Layer separation never compressed | FR-SCORE-01/03/05 | US-601/607/608 | B10/B11 | ENT-21/22 | scoring-engine | — | TC-SCORE-04 | Framework owner | 1 |
| §3.1 gates | GATE-01…07 block publication | FR-SCORE-04 | US-603 | A8 | ENT-18 | rules-gates | gate.dispositioned | TC-WF gate suite | R-CR/R-SA | 1 |
| §4 weights | 8 dims, 100 pts, core 80/module 20 | FR-SCORE-01 | US-601/609 | A13 | ENT-31 | scoring-engine | scoring_model.published | TC-SCORE-07 | Framework owner | 1 |
| §4.1 | Maturity 0–4; contribution = w×m/4; AI can't alter formulas | FR-SCORE-01/02 | US-601/602 | A7 | ENT-17 | scoring-engine | control.assessed | TC-SCORE-01; TC-AI-01 | R-FR/R-LR/R-CR | 1 |
| §4.2 | Evidence confidence A–D separate; D = no certification | FR-SCORE-03 | US-607 | B10, C4 | ENT-21 | assessment | — | AC-US-607; 22 §7 tests | Framework owner | 2 |
| §4.3 caps | CAP-00…06 incl. unreconciled ≤59, no-repayment ≤59, undisclosed = no cert | 23 §5 | US-605 | B12, A7 | ENT-21 | scoring-engine | — | TC-SCORE-03 | Framework owner | 1 |
| §4.3 overrides | Reason+evidence+approver+expiry; never silent | FR-SCORE-07 | US-606 | A9 | ENT-20 | assessment | override.* | AC-US-606 | R-SA | 1 |
| App A | 48 controls + weights as config | 21 | US-609 | A13 | ENT-31 | config-registry | scoring_model.* | weight-sum validation | Framework owner | 1 |
| App B tiers | E0–E3 evidence tiers feed confidence | 22 §3 | US-607 | A5 | ENT-13/14 | evidence | document.verified | 22 fixtures | Reviewers | 2 |
| App B contradiction | Conflicts = exceptions, never averaged | FR-AI-03 | US-503 | A6 | ENT-15 | ai-orchestrator→evidence | exception.* | AC pattern + adversarial | R-FR | 2 |
| App B N/A | Controlled taxonomy; not for missing evidence | 24 §4 | US-602 | A7/B6 | ENT-17 | rules-gates | control.assessed | TC-SCORE-06 | Framework owner | 1 |
| App B freshness | Staleness rules; expiry; re-screen at publication | FR-DOC-06, D-19 | US-405/614 | B6 | ENT-11/16 | evidence/workflow | document.expired; assessment.expired | TC freshness suite | Ops | 2 |
| App B attestation | Officer attestation pre-publication | 43 §4, 55 §1 | US-803 | B10 flow | ENT-30 | identity-access | attestation.signed | E2E J6 | R-BA/R-SA | 3 |
| §7 / §5F | KYC/UBO/sanctions human-dispositioned | 23 §2, 56 §4 | US-604/1005 | A8 | ENT-18/19 | rules-gates | screening.dispositioned | GATE-03 tests; adversarial look-alike | R-CR | 1 |
| §8 | External context reported, never blended | FR-SCORE-05 | US-608 | B10 panel | ENT-22 | assessment | — | TC-SCORE-04 | R-FR/R-SA | 1 |
| §9 | Collateral secondary to repayment | 25 §3; CAP-02 | US-605 | B5 warning | ENT-08 | scoring-engine | — | CAP-02 tests | Framework owner | 1 |
| §10/§12 | Purpose modules drive evidence | FR-WS-04 | US-304 | B6 | ENT-09 | rules-gates | — | checklist golden tests | Product | 1 |
| §11 | Docs by business type | 24/26 | US-304 | B6 | ENT-09 | rules-gates | — | profile fixtures | Framework owner | 1 |
| §14 stage 2 | Consent ledger before processing/sharing | US-1001 | US-1001/803 | B16 | ENT-24 | identity-access | consent.* | AC-US-1001 | R-BA/Compliance | 1/3 |
| §14 stage 8 | Remediation sprint + plan | 28 | US-612 | B13 | ENT-23 | assessment | — | plan generation tests | Product | 2 |
| §15 st.3–5 | Provider consented access, watermark, question log | FR-CP-01, FR-OPP-02/03 | US-804/805/806 | C1–C5 | ENT-25/26, ENT-04 | opportunity | access.*; document.downloaded | AC-US-802; watermark tests | Ops | 3 |
| §16 | Deterministic vs AI separation | 31 §2 | US-507 + arch | — | — | 41 §2 rules | — | TC-AI-01 (dependency graph) | AI lead | 2 |
| §16.1 | AI use-case matrix enforced | 31 §3 | US-501…506 | A5/A6 | ENT-12 | ai-orchestrator | ai_bundle.* | 74 §2 suite | AI lead | 2 |
| §17.1 | API data never auto-outranks signed evidence | 42 §5.1 | Phase-4 stories | A6 | ENT-15 | evidence | exception.created | integration conflict tests | R-FR | 4 |
| §18 | Model governance: versioning, validation, effective challenge | FR-SCORE-06, 31 §7–9 | US-609/507 | A13 | ENT-31 | config-registry | scoring_model.*; ai_bundle.* | TC-SCORE-02; regression gates | Framework owner/AI | 1–2 |
| §18 appeal | Borrower correction/appeal | 57 | US (57 flows) | B12/support | ENT-30 | assessment | *.corrected | 57 E2E | Ops | 2 |
| §19/§19.2 | Perimeter checks; counsel sign-off per country-product | GATE-07, 63 §3, 64 | US-1102/1301 | A14 | ENT-32 | config-registry | overlay.published | activation-block test | Compliance | 0–4 |
| §20.1 | Tokenization-compatible data standards now | 33 §1 IDs; 81 | — | — | all ENT | — | — | ID-uniqueness tests | Backend lead | 1 |
| §22.1 | Pilot metrics incl. override monitoring | 37 | US-1201–1203 | dashboards | ENT-29/30 | admin-reporting | — | metric definition tests | Product | 2–3 |
| Prompt §4.5 | Human-in-the-loop list (D-07) | FR-REV-04 etc. | US-610 | A2/A7/A8/A9/A10 | ENT-30 | workflow | all decision events | D-07 workflow suite | R-SA | 1 |
| Prompt §15 | Fees never touch scoring | FR-SCORE-10 | US-601 | — | schema-level | scoring-engine | — | TC red team 74 §3 | Founder | 1 |
| Prompt §5.5 | Restricted flags hidden lawfully | 23 §6 | US-604/802 | A8/A10 | ENT-19 | opportunity lint | flag.restricted_* | Sev-1 leak tests | R-CR/R-SA | 1/3 |

## 3. Control-block traceability (A1…H5)

All 48 controls share machinery — one row pattern, instantiated per control in the config bundle:

| Link | Value |
|---|---|
| CRF control | A1…H5 (weights per 21 §3 = CRF Appendix A) |
| Requirement | FR-SCORE-01/02; checklist links FR-WS-04 |
| Evidence types | per-control rows in 21 §3 → 26 registry |
| Story | US-601/602/304 |
| Screen | SCR-A7 (assess), SCR-B6/B11 (borrower) |
| Entity | ENT-17 (assessment), ENT-14 (evidence links), ENT-31 (definition) |
| Service | scoring-engine + rules-gates |
| Audit | control.assessed |
| Test | per-profile golden vectors (each control exercised in ≥1 vector at ≥2 maturities); anchor-text i18n tests |
| Owner | reviewer per 21 §5 |
| Phase | 1 (manual) / 2 (AI-suggested) |

Per-control verification checklist maintained as a generated artifact from the scoring-model bundle (CI job renders it; QA signs each model version).

## 4. Coverage accounting

CI job cross-references: every FR ↔ ≥1 US ↔ ≥1 AC ↔ ≥1 TC; every 14 transition ↔ workflow test; every 36 §2 event ↔ emitting code path test; every 21 control ↔ golden-vector coverage. Gaps fail the release gate (77 quality bar row "QA builds from ACs").
