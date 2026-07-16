# 64 — Regulatory and Legal Validation Checklist

| Field | Value |
|---|---|
| Purpose | The master checklist local counsel must complete per country/product before activation, plus platform-level legal items — the GATE-07 workbook |
| Audience | Counsel, compliance, founder |
| Status | Active checklist — **no item is validated until a written memo is on file** |
| Version | 1.0.0 |
| Owner | Compliance lead |
| Dependencies | 60–63 (overlays), 02 (boundaries), 06 (OQs) |
| Source references | CRF §19 (question set), §19.2 (launch posture), §0 (validation requirement) |
| Assumptions | — |
| Open questions | cross-referenced per item |
| Approval required | Risk & compliance committee accepts each memo |
| Last updated | 2026-07-16 |

## 1. Rules

1. Each item closes only with: counsel firm, memo reference, date, scope, and any conditions — recorded in the overlay `counsel.validation_items` (63 §1).
2. Product/marketing may not claim compliance, registration, or "fully legal" status based on this checklist (02 §3).
3. Re-validation triggers: fee-model change (OQ-01), new product/financing type, new user class (e.g., consumer-adjacent), law change flagged by counsel watch, 24-month age of memo.

## 2. Per-country checklist (instantiate per overlay; CRF §19 question set)

### A. Regulatory perimeter
- [ ] A1. Does any platform activity (readiness assessment, scoring, packaging, provider introduction, RFI/EOI facilitation, fee collection) constitute lending, arranging, brokerage, financial intermediation, securities placement, investment advice, crowdfunding, payments, trust/fiduciary, or credit-bureau activity? (per fee model variants — test each of OQ-01's candidates)
- [ ] A2. Product characterization: is the anticipated transaction a bilateral commercial loan / participation / note / receivables purchase / investment contract / security — and what solicitation, transfer, disclosure, or investor-eligibility rules follow for the **platform's** role?
- [ ] A3. Licensing/registration duties for the platform entity (incl. AML-registration regimes for designated non-financial activities — CR-L2 class); consequences of each revenue source (application fee, assessment fee, subscription, provider access fee, success fee) on characterization and conflicts.
- [ ] A4. Marketing/solicitation constraints (what may be said to providers/borrowers; "no pre-approval" language sufficiency).

### B. Data & privacy
- [ ] B1. Lawful basis + consent requirements for collection, screening, cross-border transfer, retention, automated analysis of borrower and **UBO/person** data (incl. registration duties with the DPA).
- [ ] B2. Cross-border transfer mechanism to chosen cloud region (OQ-04); localization constraints.
- [ ] B3. Retention minima/maxima per record class (OQ-11) + erasure carve-outs (AML vs privacy).
- [ ] B4. Breach-notification duties (authority, subjects, timelines) → feeds 45 §5 comms plan.
- [ ] B5. Automated-decision provisions vs our human-decision model (44 §4 posture confirmation).
- [ ] B6. Employee-monitoring disclosure requirements for internal security monitoring (44 §8).

### C. KYC/AML allocation
- [ ] C1. Responsibility matrix: platform vs provider vs legal/compliance partners vs payment institutions — who owes CDD, reporting, recordkeeping; reliance rules (CRF §19 "documented responsibility matrix").
- [ ] C2. Suspicious-activity reporting duties applicable to the platform (if any) + procedure (56 §3 interface).
- [ ] C3. UBO regime specifics: thresholds, registry access, acceptable evidence (GATE-02 parameters).
- [ ] C4. Sanctions obligations of the platform entity (list scope, freeze/reject duties if any).

### D. Security interests & enforcement
- [ ] D1. How liens/guarantees are created, perfected, prioritized, monitored, enforced per collateral class (H3 anchors + 25 §3 guidance).
- [ ] D2. Registry reliability + search protocols (lien search evidentiary weight).
- [ ] D3. Guarantee enforceability requirements (spousal/corporate consents, corporate benefit).

### E. Tax & FX
- [ ] E1. Withholding, stamp/registration taxes, interest-deductibility, thin-cap, transfer-pricing flags affecting typical closings → provider-package disclosure content (CR-L10 class).
- [ ] E2. Exchange-control/FX rules affecting cross-border lenders.

### F. Contracts & consumer/SME protections
- [ ] F1. Enforceability of platform terms (borrower, provider, partner agreements); liability-limitation validity (OQ-12).
- [ ] F2. E-signature validity per document class (OQ-06) — attestations, consents, agreements.
- [ ] F3. Consumer/SME protection scope: confirm business-purpose exclusion strategy (ASM-13) and any micro-enterprise protective treatment (CRF §19).
- [ ] F4. Complaint-handling / ombudsman duties applicable to the platform (57 interface).

### G. Sector & product specifics
- [ ] G1. Real-estate/construction-specific rules (permits, pre-sale regimes, escrow expectations — platform stays out of funds flow).
- [ ] G2. Any digital-asset regime separation duties (SV-L8 class; 81 boundary).

## 3. Platform-level (country-agnostic) legal items

- [ ] P1. Terms of service + provider agreement templates (54 §2 clauses) reviewed.
- [ ] P2. Reliance-limitation + disclaimer language (limitation statement ES/EN) validated (OQ-14).
- [ ] P3. E&O/professional-liability + cyber insurance placed (OQ-12; CRF §21 certification liability).
- [ ] P4. IP: framework/methodology ownership, trademark for eventual certification mark (REF-02 future).
- [ ] P5. Entity structure & inter-company arrangements (OQ-02/ASM-14).
- [ ] P6. Conflict-of-interest & fee-disclosure policy review (D-15; CRF §21 conflicts from fees).

## 4. Status board (maintained per overlay)

| Country | Perimeter memo | Privacy | AML matrix | Collateral | Tax | Contracts | Status |
|---|---|---|---|---|---|---|---|
| Costa Rica | CR-L1 open | CR-L3 open | CR-L2 open | CR-L5 open | CR-L10 open | CR-L7/L9 open | **draft — not activatable** |
| Panama | PA-L1/L2 open | PA-L6 open | PA-L5 open | PA-L8 open | PA-L9 open | PA-L7 open | pre-draft |
| El Salvador | SV-L1/L2 open | SV-L4 open | SV-L3 open | SV-L6 open | SV-L7 open | SV-L5 open | pre-draft |
