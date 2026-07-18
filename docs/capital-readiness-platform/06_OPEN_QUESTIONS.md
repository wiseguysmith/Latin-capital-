# 06 — Open Questions

| Field | Value |
|---|---|
| Purpose | Register of unresolved high-leverage questions, each with interim assumption so work is never blocked |
| Audience | Product owner, counsel, leadership |
| Status | Active log |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | 03 (assumptions), 05 (decisions absorb answers) |
| Approval required | n/a (log) |
| Last updated | 2026-07-16 |

Severity: **Blocking-launch** (must resolve before pilot), **Blocking-scale** (before Phase 3/4), **Non-blocking**.

| ID | Question | Why it matters | Interim position | Severity | Owner | Target phase |
|---|---|---|---|---|---|---|
| OQ-01 | **Resolved (D-21, 2026-07-18):** flat disclosed assessment fee paid by the business; no success/percentage/provider fees in pilot. Counsel still validates fee characterization under CR-L1 | Revenue model; regulatory characterization | — | Resolved | Founder | 0 |
| OQ-02 | **Resolved (D-22, 2026-07-18):** single operating company runs the CR pilot; local subsidiary only if counsel requires. Counsel confirms registration duties under CR-L1/CR-L2 | Legal perimeter | — | Resolved (counsel validation pending) | Counsel | 0 |
| OQ-03 | Product brand name | Marketing, domain, terms | "Capital Readiness Platform" (ASM-01) | Non-blocking | Founder | 1 |
| OQ-04 | **Resolved (D-23, 2026-07-18):** preferred position is US-region cloud with explicit cross-border consent + encryption; region stays portable (ADR-006). Counsel confirms under Ley 8968 (CR-L3) | Architecture region choice; DPA content | — | Resolved (counsel validation pending) | Counsel | 0 |
| OQ-05 | Credit-information access: can/should the platform pull CR CIC or bureau data with consent in MVP, or rely on borrower-supplied + bank statements? | Evidence tiers for debt verification | MVP relies on borrower documents + bank statements + registry/lien searches; bureau integration Phase 4 | Blocking-scale | Compliance | 2 |
| OQ-06 | E-signature validity per country for attestations and consents (qualified vs simple e-sign) | Attestation enforceability | Vendor e-sign + wet-ink fallback allowed | Blocking-launch | Counsel | 0 |
| OQ-07 | Which KYC/KYB + sanctions vendor(s) actually cover CR/PA/SV documents and lists well? | Gate operability | Manual analyst screening with official lists in Phase 1 (ASM-06) | Blocking-scale | Ops+Compliance | 2 |
| OQ-08 | Does provider-facing distribution of opportunity packages constitute securities intermediation/brokerage in any target country given fee model chosen in OQ-01? | Boundary-defining | Bilateral commercial-loan framing only; counsel memo required before Phase 3 activation | Blocking-scale | Counsel | 3 |
| OQ-09 | Should borrowers see provisional (pre-verification) scores? | Trust vs confusion; gaming | No — first score shown after human-approved assessment (ASM-16) | Non-blocking | Product | 1 |
| OQ-10 | Minimum eligibility floor for intake (revenue floor? operating history? excluded sectors list v1) | Funnel quality; prohibited-activities list needed for GATE screening | Draft exclusion list in 51 §4; revenue floor left to admin judgment in pilot | Blocking-launch | Product+Compliance | 0 |
| OQ-11 | Retention periods per record class per country (AML vs privacy tension) | 35 retention schedule has provisional values | 10-year audit/compliance, 5-year documents post-relationship, configurable per overlay | Blocking-launch | Counsel | 0 |
| OQ-12 | Insurance for the platform (E&O/professional liability, cyber) and liability limitations enforceability | CRF §21 certification liability | Terms include reliance limitations; insurance procurement pre-pilot | Blocking-launch | Founder | 0 |
| OQ-13 | Do any pilot capital providers require their own regulatory reliance conditions (e.g., bank outsourcing rules) to use the platform? | Provider onboarding contracts | Addressed per-provider in 54 diligence | Blocking-scale | Ops | 3 |
| OQ-14 | Counsel-validated Spanish wording of the limitation statement, disclaimers, consent texts, privacy notices for each country | Copy cannot ship machine-translated | English master + draft Spanish pending validation | Blocking-launch | Counsel | 1 |
| OQ-15 | Evidence-confidence thresholds and gate parameters after Phase 0 calibration (ASM-07/08 values) | Scoring model v1.0 freeze | Provisional values in 20/22 | Blocking-launch | Framework owner | 0→1 |
| OQ-16 | Adverse-media screening scope for MVP (manual search protocol vs vendor) | F2 control evidence; cost | Manual protocol defined in 51 §6 | Non-blocking | Compliance | 2 |

## Process

Answering an OQ: record the answer as a D-entry in 05, update the interim-position consumers listed in the row, then mark the OQ **Resolved (D-xx)** here (keep the row).
