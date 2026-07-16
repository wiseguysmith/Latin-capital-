# 25 — Financing Modules

| Field | Value |
|---|---|
| Purpose | Module manifests: per financing type — primary questions, required evidence, G/H emphasis, module-specific caps and monitoring expectations |
| Audience | Framework owner, reviewers, engineers (checklist + engine config) |
| Status | Draft v1.0 (scoring model v1.0.0) |
| Version | 1.0.0 |
| Owner | Framework owner |
| Dependencies | 21, 24, 26; consumed by FR-WS-04 and 20 |
| Source references | CRF §10 (capital purpose modules), §12 (docs by financing type), §9 (collateral) |
| Assumptions | Module set limited to MVP financing scope (10 §5) |
| Open questions | — |
| Approval required | Framework owner |
| Last updated | 2026-07-16 |

## 1. Module mechanics

A financing module is config: `{module_id, purpose questions, evidence manifest (EV refs, required/conditional), G/H maturity anchor specializations, module caps (CAP-04 items), monitoring package definition, applicable collateral types}`. The borrower's capital purpose (FR-WS-03) selects the module; module + business type resolve the applicability profile (24 §6). Missing **essential** module evidence triggers CAP-04 with the exact missing item disclosed (CRF §4.3).

## 2. Module manifests (from CRF §10 + §12)

### M-WC — Working capital / line
Primary questions: cash-conversion cycle, inventory/receivables build, seasonality, borrowing-base logic, short-term repayment source. **Essential evidence:** 13-week cash flow (EV-FIN-08); AR/AP/inventory aging (EV-AR-01/EV-AP-01/EV-INV-01); monthly cash forecast; bank history 12m (EV-BNK-01); draw/repayment schedule (EV-UOP-03). Conditional: purchase orders/contracts, order book, borrowing-base proposal. G emphasis: G3 alignment to cycle. H: per structure (AR-secured → H1–H4 on receivables; unsecured → R2). Monitoring: monthly (weekly-capable if AR-based).

### M-EXP — Expansion / growth term loan
Questions: incremental revenue, capacity, ramp, break-even, execution risk. **Essential:** business case + milestones (EV-UOP-01), project budget (EV-UOP-02), integrated forecast w/ sensitivity (EV-FIN-06), contracts/pipeline (EV-REV-02), equity contribution evidence (EV-UOP-04), approvals (EV-COR-03). G emphasis: G1/G2/G4. Monitoring: quarterly + milestone reports.

### M-EQUIP — Equipment finance
Questions: asset spec, vendor, delivery/installation, productivity, useful life, resale. **Essential:** vendor quote/invoice/purchase contract (EV-COL-05), asset details + useful life, insurance (EV-INS-01), ownership/registration path, ROI analysis (EV-UOP-01). Conditional: appraisal (EV-COL-02, used equipment), maintenance plan, permits. H mandatory: H1/H2 on the asset. Monitoring: annual inspection + insurance renewal.

### M-CONST — Construction / development
Questions: land/title, permits, design, contractor, budget+contingency, draw controls, pre-sales/leases, completion & takeout. **Essential:** title study (EV-PRO-01), appraisal (EV-COL-02), feasibility (EV-PRO-03), permits (EV-LIC-02), plans, construction contract/GMP (EV-PRO-04), budget + contingency (EV-UOP-02), schedule, draw package + controls (EV-PRO-05), sponsor equity, takeout/exit plan (EV-UOP-05). Conditional: quantity surveyor, pre-sales/lease contracts, environmental study (EV-ESG-02). G emphasis: G2 sources&uses + G4 takeout. H mandatory incl. H3 perfection path, H4 draw monitoring. Module caps: missing permits or title → CAP-04. Monitoring: monthly draw certification.

### M-ACQ — Acquisition finance
Questions: target quality, SPA, valuation, synergies, integration, leverage, seller exposure. **Essential:** LOI/SPA (EV-CON-02), target financials 24m+, quality-of-earnings or equivalent review (EV-FIN-09 — E3 preferred), sources & uses, pro forma (EV-FIN-06), integration plan, approvals. Conditional: diligence reports, seller financing docs. Monitoring: quarterly + covenant package.

### M-REFI — Refinancing
Questions: original purpose, payment history, maturity/covenant pressure, structural improvement, any cash-out use. **Essential:** existing agreements + statements (EV-DBT-02), payoff letters (EV-DBT-04), 12m payment history, lien releases path (EV-COL-03), old-vs-new debt-service comparison, cash-out use plan. Rule: refinancing accepted only with credible repayment strategy (10 §5); distressed-rescue framing → S2 flag + senior review. Monitoring: standard term-loan package.

### M-VD — Venture-debt-style
Questions: runway, burn, recurring revenue, retention, investor support, next raise, IP. **Essential:** cap table (EV-OWN-02), financing history + investor rights, board materials (EV-GOV-03), runway model + burn (EV-FIN-10), MRR/ARR + cohorts (EV-REV-03), IP chain (EV-IP-01). Conditional: warrant terms if proposed (platform documents, never structures — boundary 02). Profile P-STARTUP-VD substitutions (24 §5). Monitoring: monthly MRR/runway reporting capacity (G5).

### M-RBF — Revenue-based financing
Questions: revenue quality/stability, processor data, margins, seasonality, concentration, remittance burden. **Essential:** source bank/processor data (EV-BNK-01/EV-REV-04 — E2 required), refunds/chargebacks, cohort/channel analysis, remittance sensitivity stress (EV-FIN-06). H: R2 (H5 = platform/processor continuity + reserves). Monitoring: monthly revenue data.

### M-BRIDGE-RE — Real-estate bridge
Questions: as CRF §12: title/appraisal, leases, rent roll, NOI, capex plan, exit/takeout, sponsor equity, existing liens. **Essential:** title (EV-PRO-01), appraisal ≤12m, rent roll + leases (EV-PRO-02), NOI statement, capex plan, exit plan (EV-UOP-05), insurance, lien search (EV-COL-03). Monitoring: quarterly NOI + exit-milestone tracking.

### M-ABL — ABL / receivables / inventory
Questions: eligible collateral, advance rates, dilution, aging, controls, field-exam readiness. **Essential:** detailed receivables tape (EV-AR-02) or SKU-level inventory (EV-INV-02), eligibility rules, dilution analysis, debtor concentration, assignment rights (EV-CON-01), lockbox/dominion feasibility (documented path only — platform holds no funds), field-exam readiness note. H mandatory with H4 monitoring cadence weekly-capable. Monitoring: borrowing-base frequency per structure.

### M-CONTRACT — Contract / project-revenue financing
Questions: contract quality/enforceability, counterparty credit, milestones, performance risk. **Essential:** the contract(s) (EV-CON-02), counterparty due diligence summary, milestone/billing schedule, performance history, bonding/insurance where applicable. Monitoring: per-milestone.

## 3. Collateral-type diligence (CRF §9 — feeds H controls across modules)

| Collateral | Core diligence | Monitoring/haircut drivers |
|---|---|---|
| Real estate | Title, survey, zoning, permits, appraisal, environmental, taxes, leases, encumbrances, insurance, foreclosure path | location, liquidity, completion, tenancy, priority, appraisal age, disaster risk |
| Equipment | Ownership, serials, condition, age, maintenance, OLV appraisal, location, insurance | obsolescence, mobility, specialized use, resale market |
| Receivables | Invoice validity, debtor credit, aging, disputes, offsets, dilution, concentration, assignment rights | eligibility, confirmations, dominion, borrowing-base frequency |
| Inventory | Ownership, location, type, age, turnover, marketability, valuation, storage, insurance | perishability, obsolescence, seasonality, WIP, liquidation cost |
| IP | Chain, registrations, licenses, infringement, revenue linkage, valuation method | valuation uncertainty, team/platform dependency, enforceability |
| Guarantees | Guarantor identity/assets/liabilities, capacity, waivers, corporate benefit, enforceability | guarantor liquidity, competing claims, jurisdiction, revocation |
| Cash/reserves | Account ownership, control agreement, permitted investments, release conditions | bank risk, set-off, currency, replenishment |

Collateral principle (CRF §9): collateral is a **secondary** repayment source; an attractive appraisal never compensates for weak ownership/enforceability/monitoring or an implausible primary source — enforced via B1/G3 gates on repayment logic (CAP-02) independent of H scores.
