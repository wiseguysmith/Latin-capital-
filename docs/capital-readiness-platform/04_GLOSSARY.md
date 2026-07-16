# 04 — Glossary

| Field | Value |
|---|---|
| Purpose | Single canonical vocabulary; all documents and product copy must use these terms |
| Audience | All |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | none |
| Source references | CRF v1.0 throughout; Master prompt |
| Approval required | Product owner |
| Last updated | 2026-07-16 |

Terms are grouped; **bolded terms** are the canonical form. Do not use listed synonyms in UI copy.

## Methodology

- **Capital Readiness Framework (CRF)** — the controlling methodology document (v1.0). Not a lending policy, not legal advice.
- **Capital Readiness Score** — deterministic 0–100 score = Core Score (80) + Financing Module Score (20). *Not*: credit score, rating, PD. Synonyms to avoid: "credit readiness", "trust score".
- **Core Score** — 80-point borrower-level layer: dimensions A–F.
- **Financing Module Score** — 20-point deal-specific layer: dimensions G–H, shaped by the selected **Capital Purpose Module**.
- **Dimension** — one of eight scored categories A–H (see 21). "Category" in the CRF = "dimension" here.
- **Control** — one of 48 assessable items (A1…H5) with weight, maturity anchors, applicability, freshness, and red-flag triggers.
- **Maturity level** — 0 Absent / 1 Informal / 2 Documented / 3 Implemented / 4 Institutional (CRF §4.1).
- **Control contribution** — `weight × maturity ÷ 4`.
- **Readiness band** — 90–100 Institutional Ready; 75–89 Capital Ready; 60–74 Conditionally Ready; 40–59 Developing; 0–39 Not Ready.
- **Evidence Confidence grade** — A High / B Good / C Limited / D Insufficient; computed separately from the score (22).
- **Evidence tier** — E0 Unsubstantiated / E1 Borrower document / E2 Source-system or counterparty / E3 Independent professional (CRF Appendix B).
- **External Risk Context** — reported 1–5 profile (Low→Severe) of exogenous risks; never a hidden score penalty (CRF §8.1).
- **Gate (eligibility & integrity gate)** — pass/fail/enhanced-review precondition (GATE-01…07); failing blocks scoring publication (23).
- **Score cap** — deterministic maximum applied on defined conditions (CAP-01…; e.g., unreconciled financials → max 59).
- **Red flag** — a recorded material issue with severity S1–S4, resolution owner, and visibility rules (23).
- **Condition** — a disclosed requirement attached to an approved assessment ("Approved with conditions").
- **Override** — a human-approved deviation from the system result, with reason, evidence, named approver, timestamp, expiry; never silently replaces the system score (23 §7).
- **N/A (non-applicable) control** — excluded via the controlled applicability taxonomy only; never because evidence is missing (24).
- **Scoring model version** — immutable versioned bundle of controls, weights, applicability rules, caps, band edges, and confidence thresholds (20 §8).
- **Preliminary assessment** — assessment computed before the minimum core dataset is complete; internal-facing, never publishable (REF-05).
- **Readiness Report** — borrower-facing output artifact (27). The provider-facing variant is the **Readiness Package**. "CRF Certificate" is reserved post-pilot (REF-02).
- **Gap-remediation plan / Readiness Action Plan** — prioritized actions to reach the next band (28).

## Product & workflow

- **Applicant** — a business in the intake funnel before acceptance (states: Draft…Rejected/Withdrawn, 14 §2).
- **Borrower** — accepted business organization. (CRF says "borrower"; UI uses "business" when addressing them.)
- **Capital provider** — approved lender/investor organization. CRF's "lender" maps here; use "capital provider" in product copy.
- **Capital workspace** — the borrower's working environment (profile, checklist, document room, tasks, results) (D-03).
- **Document room** — the secure evidence vault within a workspace; the provider-visible subset is the **approved document room**.
- **Personalized checklist** — evidence requirements generated from business type × financing type × jurisdiction × amount (24, 26).
- **Clarification request** — a structured question to the borrower raised by AI (draft) or reviewer (approved) about evidence.
- **Verification** — human confirmation of AI-extracted or borrower-declared data (52).
- **Assessment** — one scored evaluation lifecycle instance (14 §3).
- **Opportunity** — a packaged, approved assessment prepared for capital-provider access (14 §5).
- **Publication** — granting named providers access to an opportunity after senior approval + borrower consent (55).
- **RFI (request for information)** — provider question routed through the platform.
- **EOI (expression of interest)** — provider's non-binding indication; never an approval.
- **Attestation** — authorized officer's signed confirmation of completeness/accuracy and duty to disclose changes (CRF §14 stage 9).

## Roles (canonical IDs in 11)

Prospective applicant (R-PA), Business administrator (R-BA), Business team member (R-BM), Internal platform administrator (R-IA), Application reviewer (R-AR), Financial reviewer (R-FR), Legal reviewer (R-LR), Compliance reviewer (R-CR), Senior approver (R-SA), Capital-provider administrator (R-CPA), Capital-provider analyst (R-CPN), Legal partner (R-LP), Compliance partner (R-CP), Accounting/financial-review partner (R-AP), Read-only auditor (R-AUD), Support user (R-SUP).

## AI & data

- **Per-document analysis** — pipeline run after each upload: classify → extract → map to controls → detect defects → draft clarifications → provisional evidence confidence → route to verification (D-04).
- **Full-package analysis** — cross-document reconciliation, contradiction and staleness detection, completeness, deterministic scoring, explanation drafts (D-04).
- **Extraction schema** — versioned structured-output contract per document type (32).
- **Field-level provenance** — source document, page/region, extractor, confidence, verifier, timestamp for every material field (CRF §16 layer 2).
- **Contradiction / exception** — material conflict between evidence items; never averaged; always an exception requiring human resolution (CRF Appendix B).
- **Verification queue** — human work queue for low-confidence/material AI outputs.
- **Deterministic service** — code path with no ML/LLM in the decision loop.
- **Audit event** — immutable record `actor + action + object + timestamp + context` (36).

## Compliance & legal

- **KYB / KYC** — business / individual identity verification. **UBO** — ultimate beneficial owner (natural person). **PEP** — politically exposed person.
- **Sanctions screening** — matching parties against UN/OFAC/EU/local lists; matches are dispositioned only by humans (R-CR).
- **Consent ledger** — record of each data-use/sharing authorization, scope, basis, timestamp, and revocation.
- **Jurisdictional overlay** — versioned country configuration (evidence, identity fields, entity types, language, disclaimers…) (63).
- **Regulatory perimeter** — the licensed/permitted scope of platform activity per country; changes require counsel sign-off (64).
- **Legal hold** — suspension of deletion/retention rules for specified records (35).

## Explicit non-equivalences (test-enforced; 73 §6)

- Capital Readiness Score ≠ credit score ≠ default probability ≠ approval.
- Evidence Confidence ≠ readiness; ≠ audit.
- EOI ≠ term sheet ≠ commitment.
- Acceptance into platform ≠ funding approval ≠ underwriting decision.
- External Risk Context ≠ borrower quality.
