# 26 — Document and Evidence Taxonomy

| Field | Value |
|---|---|
| Purpose | Canonical evidence-type registry: applicability, status, formats, periods, ages, signatures, verification, extraction fields, controls, defects, privacy, retention, sharing |
| Audience | Framework owner, reviewers, engineers (checklist/extraction config), designers |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Framework owner |
| Dependencies | 21 (controls), 22 (tiers/freshness), 24 (profiles), 32 (schemas), 35 (retention), 44 (privacy classes) |
| Source references | CRF §11 (by business type), §12 (by financing type), Appendix B; master prompt §11 |
| Assumptions | ASM-07 (ages ◆ where CRF qualitative), ASM-11 (formats/sizes) |
| Open questions | OQ-11 (retention per country) |
| Approval required | Framework owner |
| Last updated | 2026-07-16 |

## 1. Registry model

```
EvidenceType {id, name, category, description,
 borrower_types[], financing_modules[], status: required|conditional|optional (per profile),
 formats[], reporting_period, max_age_days, signature_req,
 verification_method (tier path E1→E2/E3), extraction_schema (32),
 related_controls[], common_defects[], replacement_rules,
 privacy_class P1–P4 (44), retention_class (35), sharing_rule}
```
Overlays may add jurisdiction-specific types or field variants (63). "Verification method" defines how E1 uploads can be upgraded (registry cross-check, bank-data match, professional confirmation).

## 2. Universal core file (CRF §11 — required for every borrower)

| ID | Evidence type | Formats | Period / max age◆ | Sig | Verification path | Key extraction fields (32) | Controls | Privacy | Sharing default |
|---|---|---|---|---|---|---|---|---|---|
| EV-COR-01 | Formation deed / bylaws / operating agreement | PDF | current version | notarized | registry cross-check (CR: Registro Nacional) → E2/E3 | entity name, id, type, date, powers, signatories | E1 | P2 | approved room |
| EV-COR-02 | Good standing / literal registry certificate | PDF | ≤30d at approval | issuer | registry verification → E3 | status, issue date, registry no. | E1, GATE-01 | P2 | approved room |
| EV-COR-03 | Board/shareholder authorizations | PDF | matter-specific | signed | legal review → E2 | resolution scope, date, signers | E1, C2, G6 | P2 | approved room |
| EV-COR-04 | Shareholder/UBO registers | PDF/XLSX | ≤90d | signed | registry/RTBF cross-check (CR) → E2/E3 | holders, %, classes | E2c, GATE-02 | P4 | summary only |
| EV-OWN-01 | Ownership/group chart + UBO chain | PDF/structured | ≤90d | R-BA attested | compliance verification | entities, persons, %, chain | E2c, C4, F1 | P4 | summary only |
| EV-OWN-02 | Cap table (startups: full history) | XLSX/structured | ≤90d | attested | financing docs cross-check | rounds, holders, instruments | C4, E2c | P4 | summary only |
| EV-OWN-03 | UBO identity documents | PDF/IMG | valid (unexpired) | — | KYC verification (ASM-06) | name, doc no., DOB, nationality | F1, GATE-02 | P4 | never (internal) |
| EV-REL-01 | Related-party schedule | XLSX | ≤90d | attested | cross-check vs financials | parties, nature, amounts | C4, A3 | P3 | approved room (redacted persons) |
| EV-FIN-01 | Annual financial statements (24–36m) | PDF/XLSX | ≤FY+9m | accountant/auditor | reconciliation + professional status check → E2/E3 | full statement schema 32 §3 | A1, B* | P3 | approved room |
| EV-FIN-03 | YTD management accounts | PDF/XLSX | ≤60d | mgmt | reconciliation | P&L/BS/CF fields | A2, D3 | P3 | approved room |
| EV-BNK-01 | Bank statements (12m all material accounts) | PDF/CSV | ≤90d | bank-issued | inflow/outflow reconciliation → E2 | account, period, balances, flows | A3, B1, B2 | P3 | summary/approved per R-SA |
| EV-TAX-01 | Tax filings (income/VAT, 24m) + payment status | PDF | last filings; status ≤90d | filed | tax-authority verification where available → E2 | period, base, tax, payment | A3, F5 | P3 | approved room |
| EV-DBT-01 | Debt & guarantees schedule | XLSX/structured | ≤90d | attested | agreements + lien search cross-check | lender, balance, currency, rate, maturity, amortization, collateral, covenants, arrears (CRF §6.1) | B4, E3 | P3 | approved room |
| EV-DBT-02 | Loan/credit agreements | PDF | current | signed | legal review | parties, terms, covenants, security | E3c, B4, G6 | P3 | approved room |
| EV-AR-01 / EV-AP-01 | AR / AP aging | XLSX | ≤60d | system export | reconciliation to BS | buckets, top debtors/creditors | B5, A3 | P3 | approved room |
| EV-CON-01 | Material contracts | PDF | current | signed | legal review | parties, term, value, CoC/assignment clauses | E4 | P2/P3 | approved room (selected) |
| EV-UOP-01/02/03 | Use of proceeds / sources & uses / repayment plan | structured+PDF | current request | attested | financial review | line items, amounts, schedule | G1–G3 | P3 | approved room |
| EV-FIN-06 | Integrated forecast + base/downside | XLSX | ≤90d | mgmt | assumption review | assumptions, cases, monthly CF | A4, B6, G4 | P3 | approved room |
| EV-LIC-01 | Licenses/permits/registrations | PDF | current, unexpired | issuer | issuer/registry check → E2 | scope, expiry, conditions | E5 | P2 | approved room |
| EV-LEG-01 | Litigation & claims schedule (+counsel letter where available) | PDF | ≤90d | counsel preferred | judicial-registry check where feasible → E2/E3 | matters, status, exposure | E6 | P3 | approved room |
| EV-INS-01 | Insurance policies/certificates | PDF | current | insurer | insurer confirmation | coverage, limits, expiry, beneficiary | D5, H4 | P2 | approved room |
| EV-CMP-01/02/03/04 | KYB/KYC file; screening results; source-of-funds; anti-corruption policy/diligence | PDF/structured | per ASM-07 | — | compliance verification | per 32 §6 | F1–F4, GATE-02/03 | P4 | never (summary only) |
| EV-GOV-01…05 | CVs, governance docs, minutes, succession, authority matrix | PDF | ≤12m | varies | legal/reference review | roles, structures | C1–C5 | P2/P4(CVs) | selected |
| EV-OPS-01…05 | Process/KPI docs, internal-control docs, ops reports, IT/security summary, BCP | PDF | ≤12m | mgmt | reviewer assessment | — | D1–D5 | P2 | selected |
| EV-ESG-01 | E&S materiality screen + sector permits | PDF/structured | ≤12m | mgmt | reviewer/partner | sector risks, permits, incidents | F6 | P2 | approved room |
| EV-LAB-01 | Social-security/labor compliance evidence (CR: CCSS status) | PDF | ≤90d | issuer | authority verification → E2 | status, arrears | F5 | P3 | approved room |

### 2b. Registry completion — additional evidence types referenced by controls/modules (21, 25)

Compact definitions; each follows the §1 registry model with default privacy P3 (P2 where noted) and approved-room sharing unless stated.

| ID | Evidence type | Notes (period/max age◆, verification path, controls) |
|---|---|---|
| EV-FIN-02 | Audit/review report & notes to financial statements | with EV-FIN-01; auditor status check → E3; A1 |
| EV-FIN-04 | Reconciliation workpapers (statements↔bank↔tax↔ledgers) | ≤90d; R-FR review; A3 |
| EV-FIN-05 | Normalized-earnings bridge (owner comp, one-offs, related parties — CRF §6.1) | ≤90d; B3 |
| EV-FIN-07 | Data-lineage register (source system, owner, extraction dates) | at assessment; A5 |
| EV-FIN-08 | 13-week cash-flow forecast | current; M-WC essential; B2/G3 |
| EV-FIN-09 | Quality-of-earnings or equivalent independent review | ≤6m; E3; M-ACQ |
| EV-FIN-10 | Runway/burn model | ≤60d; BT-STARTUP; B1-substitute |
| EV-REV-01 | Revenue detail by customer/month (24–36m) | ≤90d; source-system export → E2; B5 |
| EV-REV-02 | Contracts/pipeline/order book | ≤90d; B5, M-EXP |
| EV-REV-04 | Processor/marketplace reports | ≤60d; E2 required for M-RBF |
| EV-DBT-03 | Guarantees & contingent-obligations schedule | ≤90d; attested; E3c/B4 |
| EV-DBT-04 | Payoff letters / lien-release path | ≤30d at approval; M-REFI |
| EV-OWN-04 | Shareholder/partners agreements | current; legal review; C4 |
| EV-AR-02 | Detailed receivables tape (invoice-level) | ≤30d; E2 export; M-ABL/H |
| EV-AP-01 | AP aging (defined §2 with EV-AR-01) | — |
| EV-INV-01 | Inventory aging summary | ≤60d; BT-ASSET/M-WC |
| EV-INV-02 | SKU-level inventory report | ≤30d; M-ABL inventory |
| EV-COL-01 | Collateral asset identification file (titles, serials, registry ids) | ≤90d; registry cross-check → E2; H1 |
| EV-COL-02 | Independent appraisal (defined §2 exemplar) | RE ≤12m, equip/inv ≤6m; E3; H2 |
| EV-COL-03 | Lien/registry search report | ≤30d at approval; E2/E3; H3, E3c |
| EV-COL-04 | Collateral monitoring reports (borrowing base, inspections) | per module cadence; H4 |
| EV-COL-05 | Vendor quote/invoice/purchase contract (equipment) | current; M-EQUIP; H1 |
| EV-COL-06 | Fixed-asset register + maintenance records | ≤6m; BT-ASSET; H1/H4 |
| EV-UOP-04 | Equity/sponsor contribution evidence | ≤60d; bank/e2 trail; G2 |
| EV-UOP-05 | Exit/takeout/refinance plan | current; G4; M-CONST/M-BRIDGE-RE |
| EV-CON-02 | Transaction contracts (LOI/SPA, off-take, project contracts) | current; legal review; M-ACQ/M-CONTRACT |
| EV-LEG-02 | Counsel letters / legal opinions | matter-specific; E3; E6/H3 |
| EV-IP-01 | IP registrations, assignments, license agreements | ≤12m; registry check where applicable; E7 |
| EV-TAX-02 | Tax-dispute/arrears documentation | ≤90d; authority verification; E6/F5 |
| EV-PRO-02…05 | Rent roll+leases; feasibility study; construction contracts/GMP; draw package & controls | per M-CONST/M-BRIDGE-RE manifests (25); P2/P3 |
| EV-ESG-02 | Environmental study/permit (e.g., SETENA viability in CR) | project-stage-specific; issuer verification → E2; F6/M-CONST |
| EV-LIC-02 | Zoning/land-use/construction permits | current; issuer/registry → E2; E5/M-CONST |
| EV-STARTUP set | financing docs, investor-rights, board materials (EV-GOV-03), vesting schedules | per 24 §5; C-dim + M-VD |

## 3. Type- and module-specific sets (composition per 24 §6 profiles; sources CRF §11–12)

- **BT-STARTUP add:** EV-OWN-02 history, financing docs, board materials (EV-GOV-03), runway/burn model (EV-FIN-10), MRR/ARR + cohorts (EV-REV-03), IP chain (EV-IP-01), founder vesting, next-raise milestones.
- **BT-RE-DEV/M-CONST add:** title study (EV-PRO-01), appraisal (EV-COL-02), zoning/permits (EV-LIC-02), plans, feasibility (EV-PRO-03), construction contracts (EV-PRO-04), budget/contingency, schedule, draw package (EV-PRO-05), pre-sales/leases (EV-PRO-02), environmental (EV-ESG-02), sponsor-equity proof (EV-UOP-04), takeout (EV-UOP-05).
- **BT-RE-OP/M-BRIDGE-RE add:** rent roll + leases (EV-PRO-02), NOI statements, capex plan, property tax/insurance, lien search (EV-COL-03).
- **BT-ASSET add:** fixed-asset register (EV-COL-06), maintenance records, capacity utilization, safety/environmental permits, equipment appraisal, SKU inventory (EV-INV-02).
- **BT-AR/M-ABL add:** receivables tape (EV-AR-02), eligibility/dilution analysis, debtor confirmations, assignment rights, warehouse/lockbox controls docs.
- **M-ACQ add:** LOI/SPA (EV-CON-02), target financials, QoE (EV-FIN-09), integration plan, pro forma.
- **M-REFI add:** payoff letters (EV-DBT-04), payment histories, lien releases, structure comparison.
- **Exporter tag add:** customs records, FX exposure/hedging docs, Incoterms contracts, LC history, sanctions screening of counterparties.
- **Agriculture tag add:** land/use rights, crop/water data, off-take contracts, weather/insurance, biosecurity.

## 4. Guarantees & alternative support

EV-GTE-01 guarantee + guarantor financial file (guarantor treated as linked party: identity, assets/liabilities statement, capacity evidence, spousal/corporate consents per overlay). Controls H5/H1–H3; privacy P4 for personal guarantors; sharing summary-first.

## 5. Common-defect taxonomy (rejection reasons, FR-DOC-04)

| Code | Defect | Typical fix guidance |
|---|---|---|
| DEF-01 | Illegible / poor scan | re-scan ≥300dpi |
| DEF-02 | Missing pages/sections | upload complete document |
| DEF-03 | Wrong period | correct period per checklist item |
| DEF-04 | Unsigned / uncertified | obtain signature/certification |
| DEF-05 | Expired | obtain current issuance |
| DEF-06 | Wrong entity (name/ID mismatch) | provide doc for the applicant entity |
| DEF-07 | Inconsistent with other evidence | see clarification thread (never averaged) |
| DEF-08 | Untranslated where certified translation required | certified translation, original preserved (CRF App B) |
| DEF-09 | Format not extractable/acceptable | resubmit per format guidance |
| DEF-10 | Suspected alteration | escalated (GATE-04 path) — borrower messaging neutral: "verification issue" |

## 6. Replacement, renewal, translation

New versions supersede (14 §4); expiring items prompt at T-14d (FR-DOC-06); originals always preserved alongside translations; machine translation permitted for reviewer convenience but labeled and never legally relied upon (CRF App B).

## 7. Privacy, retention, sharing summary

Privacy classes and handling: 44 §3. Retention classes: 35 §3 (OQ-11 pending counsel). Sharing defaults above are overridable only downward (more restrictive) by R-BA consent scope or R-SA packaging decisions; upgrades (e.g., sharing UBO identity docs) are never permitted in provider rooms (12 §3).
