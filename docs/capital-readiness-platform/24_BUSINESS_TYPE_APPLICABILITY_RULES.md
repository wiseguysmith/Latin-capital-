# 24 — Business-Type Applicability Rules

| Field | Value |
|---|---|
| Purpose | How controls, evidence, and scoring adapt by business type, maturity, and financing structure: applicability profiles, N/A taxonomy, weight reallocation |
| Audience | Framework owner, backend engineers, reviewers |
| Status | Draft v1.0 (scoring model v1.0.0) |
| Version | 1.0.0 |
| Owner | Framework owner |
| Dependencies | 20 (engine), 21 (controls), 25 (modules), 26 (evidence) |
| Source references | CRF §11 (docs by business type), §13.1 (personas), Appendix B (N/A + reallocation rules); D-16 |
| Assumptions | Reallocation maps are v1.0.0 proposals within CRF App B's "approved module rules" requirement |
| Open questions | OQ-15 |
| Approval required | Framework owner + R-SA |
| Last updated | 2026-07-16 |

## 1. Business-type taxonomy (D-16)

| ID | Type | CRF §11 anchor | Notes |
|---|---|---|---|
| BT-SME | Established operating SME / family business | "Established SME / family business" | default profile |
| BT-STARTUP | Startup / pre-profit / emerging | "Startup / pre-profit" | metric substitutions §3.2 |
| BT-RE-DEV | Real-estate developer / project SPV | "Real estate owner / developer / project SPV" | pairs with construction/bridge modules |
| BT-RE-OP | Real-estate operating company | same row (operating emphasis) | rent roll/NOI emphasis |
| BT-ASSET | Asset-heavy (manufacturing/logistics/agri) | "Manufacturing / logistics / asset-heavy", "Agriculture / food" | sector tags refine evidence |
| BT-AR | Receivables-backed borrower | "Commerce", "Professional services" + ABL emphasis | pairs with ABL/receivables module |
| BT-PROJECT | Project-finance-style opportunity | construction/project rows | contracted-cash-flow emphasis |

Sector tags (secondary): saas, ecommerce, services, exporter/importer, agriculture — pull extra evidence rows from CRF §11 (26 §3).

## 2. Applicability profile mechanics

`ApplicabilityProfile = f(business_type, financing_module, collateral_structure)` → for each control: `applicable | conditional | not_applicable(default_reason)`, plus a **weight reallocation map**. Profiles are scoring-model config; the checklist generator (FR-WS-04) and engine (20 §4 step 1) both consume them, so evidence requirements and scoring stay in lockstep.

## 3. Reallocation rules (CRF App B: "weights may be redistributed only within approved module rules and recorded")

- **R1 (within-dimension):** N/A control weight redistributes **pro-rata across remaining applicable controls in the same dimension**. A dimension never changes its total weight (core stays 80, module stays 20 — CRF §3.2 comparability).
- **R2 (H-dimension unsecured):** where the financing is unsecured/alternative-support-based, H1–H4 = N/A and H5 carries the full 6.0 (21 §3.H).
- **R3 (floor):** a dimension may not have fewer than 2 applicable controls unless an explicit named exception exists in the profile (currently only H under R2).
- Every reallocation is recorded in the assessment result (auditable; CRF App B "Reallocation… recorded").

## 4. N/A taxonomy (controlled; CRF App B: "never used merely because evidence is missing")

| Code | Reason | Example | Approval |
|---|---|---|---|
| NA-STRUCT | Structurally inapplicable to financing structure | H1–H4 for unsecured deal | automatic via profile |
| NA-TYPE | Inapplicable to business type | E7 IP for a land-holding SPV with no IP | profile default, reviewer confirm |
| NA-STAGE | Inapplicable to company stage | B4 Debt/EBITDA for pre-revenue startup (substituted, §3.2 below — prefer substitution over N/A) | reviewer + framework-owner-approved substitution |
| NA-JUR | Jurisdictionally inapplicable | overlay-specific | overlay config |
| NA-REQ | Reviewer-ruled with justification | edge cases | dimension reviewer + R-SA |

Missing evidence is always maturity 0 (or a gate/cap), never N/A. Borrower "mark not applicable" on conditional checklist items (SCR-B6) creates an NA-REQ proposal requiring reviewer confirmation.

## 5. Metric & evidence substitutions by type (CRF §2 venture-debt row, §6 cautions, §11)

### BT-STARTUP (avoid forcing EBITDA frameworks — CRF §2)
- B1 primary repayment → runway + milestone-based repayment logic + recurring-revenue quality (MRR/ARR, retention/cohorts) + investor support evidence.
- B4 leverage → burn multiple/liability schedule; Debt/EBITDA marked NA-STAGE with substitution note.
- Extra evidence: cap table, financing history, board approvals, investor rights, founder vesting, IP chain, next-raise milestones (EV-STARTUP set, 26 §3).
- E7 IP weight in practice raised via reallocation only when other E controls are N/A — otherwise emphasis handled through maturity anchors, not weight edits (keeps comparability).

### BT-RE-DEV / BT-PROJECT
- B-dimension focus: project cash flows, pre-sales/leases, sponsor equity; B5 revenue stability → contracted-revenue quality.
- H mandatory (no R2); H2 appraisal ≤12m; construction module manifest (25) drives G/H evidence (budget, contingency, draw controls, takeout).
- D5 continuity → contractor performance security, builder's risk insurance.

### BT-RE-OP
- B1 → NOI/DSCR on rent roll; evidence: leases, rent roll, occupancy history; H2 appraisal + tax/insurance.

### BT-ASSET
- H emphasis on registers/maintenance/appraisals; D2 inventory & fixed-asset controls; sector permits under E5/F6.

### BT-AR
- B5+H: receivables tape, aging, dilution, debtor concentration, assignment rights, verification/confirmations; monitoring cadence (H4) weekly-capable.

## 6. Profile matrix v1.0.0 (initial shipped set)

| Profile ID | Type × Module | Notable N/A | Reallocation |
|---|---|---|---|
| P-SME-WC-UNSEC | BT-SME × working capital, unsecured | H1–H4 | R2 |
| P-SME-WC-AR | BT-SME × working capital, AR-secured | — | — |
| P-SME-TERM-EQUIP | BT-SME × equipment | — | — |
| P-SME-REFI | BT-SME × refinancing | — | — |
| P-STARTUP-VD | BT-STARTUP × venture-debt-style | B4 subst.; H per structure | R1/R2 |
| P-STARTUP-RBF | BT-STARTUP × revenue-based | B4 subst.; H1–H4 → H5 | R2 |
| P-REDEV-CONST | BT-RE-DEV × construction/development | — | — |
| P-REOP-BRIDGE | BT-RE-OP × RE bridge | — | — |
| P-AR-ABL | BT-AR × ABL/receivables | — | — |
| P-ASSET-INV | BT-ASSET × inventory finance | — | — |
| P-PROJECT-GEN | BT-PROJECT × project finance | — | — |
| P-SME-EXP | BT-SME × expansion term loan | H per structure | R1/R2 |

Each ships with: control applicability list, evidence manifest (26), module manifest (25), and ≥1 golden scoring vector (20 §11). New profiles = MINOR model version.

## 7. Amount-band adjustments

Amount bands (<$500k exception-only; $500k–$1M; $1M–$2M; >$2M) tune evidence depth, not weights: higher bands require higher tiers (e.g., >$1M: reviewed/audited financials preferred for A1 maturity 4; >$2M: E3 verification on ownership and debt). Config lives in overlay + profile manifests.
