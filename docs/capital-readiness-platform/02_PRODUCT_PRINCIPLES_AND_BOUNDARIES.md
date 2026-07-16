# 02 — Product Principles and Boundaries

| Field | Value |
|---|---|
| Purpose | Binding principles and prohibited behaviors that constrain every feature, screen, message, and contract |
| Audience | Everyone; mandatory reading before contributing to product, copy, or code |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product + Compliance lead (joint) |
| Dependencies | Controls all other documents |
| Source references | CRF §0 (design boundary), §1, §3, §16, §19.2, §21; Master prompt §1, §4.7, §5 |
| Assumptions | None — this document only restates controlling sources |
| Open questions | OQ-02 (platform entity licensing posture per country) |
| Approval required | Founder + external counsel review of §3 language rules |
| Last updated | 2026-07-16 |

## 1. Principles (positive obligations)

| # | Principle | Practical consequence |
|---|---|---|
| P1 | **Readiness, not creditworthiness.** We measure preparedness of controllable factors. | Score UI, reports, certificates, and marketing must carry the limitation statement (§4). Providers own underwriting. |
| P2 | **Layer separation.** Gates, core score, financing module, evidence confidence, external context, flags/overrides, human review, and underwriting are distinct layers (CRF §3). | No feature may compress these into one unexplained number. Each layer has its own data structures (33) and UI surfaces (18). |
| P3 | **Deterministic math, assistive AI.** All formulas, weights, gates, caps, permissions, and state transitions run in deterministic services (CRF §16). | AI outputs are proposals routed to humans or to deterministic validators; see 31 §3. |
| P4 | **Human accountability.** Named humans approve every material outcome (D-07). | Every approval records actor, timestamp, reason, and evidence in the audit log (36). |
| P5 | **Evidence over assertion.** Claims score only as well as their evidence tier, freshness, and reconciliation (CRF Appendix B). | Questionnaire-only answers max out at maturity 1 for most controls (21). |
| P6 | **Consent-gated sharing.** No borrower data reaches a capital provider without explicit, recorded, scoped consent (CRF §19.2). | Publication SOP 55; consent ledger ENT-24. |
| P7 | **Explainability.** Every score, flag, and cap is traceable to controls, evidence, and rules. | Reason codes required on all adverse or limiting outcomes; borrowers see correction paths (57). |
| P8 | **Jurisdiction as overlay.** Country rules are configuration validated by counsel (60–63). | No country-specific hardcoding in core services. |
| P9 | **Manual-first.** A process is automated only after it works manually (Phase 0/1). | SOPs 51–56 are executable without automation. |
| P10 | **Score integrity is independent of revenue.** Fees, subscriptions, or success fees never influence scoring, review priority for score outcomes, or overrides (D-15). | Billing systems have no write path into scoring; analysts are never compensated on score uplift (CRF §21). |

## 2. Prohibited platform behaviors (hard boundaries)

The platform, in MVP and until an explicit licensed change of posture, must **not**:

1. Lend money or take balance-sheet credit exposure.
2. Collect, hold, custody, transmit, or settle borrower or investor funds.
3. Custody securities; issue a security; tokenize an asset; operate an exchange or secondary market.
4. Execute loan agreements automatically or bind any party to a transaction.
5. Approve credit or investments; promise, imply, or advertise funding, rates, or "pre-approved" status.
6. Provide legal, tax, accounting, investment, or credit advice (coaching content must remain non-advisory; see §3).
7. Clear a sanctions match, make a fraud determination, or make a legal determination by automation.
8. Share a data room or borrower identity with a provider without recorded consent.
9. Let payment status alter a readiness result, flag, gate, or queue position affecting score outcomes.
10. Use protected-class attributes in scoring or matching (except where law requires collection for compliance, stored segregated; 44).

The platform **may**: coordinate introductions, workflows, information, document requests, and approved service providers; operate consent-based matching with explainable, non-binding rankings.

## 3. Language and claims rules (product copy, reports, sales)

**Prohibited terms** applied to a scored opportunity: "approved", "pre-approved", "safe", "investment grade", "credit rating", "guaranteed", "certified creditworthy", "low default risk", "recommended investment".
**Required framing:** "capital readiness", "preparedness", "evidence confidence", "opportunity is organized, evidenced, and reviewable" (CRF §0).
**Mandatory limitation statement** (must appear on: score screen SCR-B10, readiness report, CRF certificate, provider opportunity profile SCR-C3, any exported package):

> The Capital Readiness Score measures how prepared this business is for capital review. It is not a credit score, a probability of default, a loan approval, an investment recommendation, or a guarantee of funding. Capital providers must perform their own independent underwriting, diligence, and approval process.

Spanish-language equivalent must be validated by counsel (OQ-14) — machine translation of the limitation statement is not acceptable for production.

## 4. Boundary enforcement mechanisms

| Mechanism | Where specified |
|---|---|
| Role/permission model prevents out-of-boundary actions | 12, 43 |
| Workflow engine blocks transitions lacking human approval | 14 |
| AI allow/deny matrix enforced at service layer, not prompt layer | 31 §3 |
| Copy lint list (prohibited terms) in CI for frontend strings | 73 §6 |
| Provider terms of use require independent underwriting attestation | 54 |
| Audit log immutability | 36, 44 |
| Counsel sign-off gate before activating any country/product combination | 64 |

## 5. Decision rights

| Decision | Owner | Cannot be delegated to |
|---|---|---|
| Methodology change (weights/bands/gates/caps) | Framework owner + senior approver, per 20 §10 | Engineering, AI, sales |
| Applicant acceptance | Internal admin/application reviewer | AI, borrower success staff |
| Sanctions disposition | Compliance reviewer | AI, any single junior reviewer |
| Final assessment approval | Senior approver | The analyst who prepared it (segregation, 12 §6) |
| Publication to providers | Senior approver + borrower consent | Provider demand, sales |
| Boundary changes (this document) | Founder + counsel | Anyone else |
