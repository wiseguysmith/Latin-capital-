# 21 — Control Library and Weighting Model

| Field | Value |
|---|---|
| Purpose | The 45-control registry (weights from CRF Appendix A), maturity anchors, evidence expectations, freshness, red-flag triggers, and storage model |
| Audience | Framework owner, reviewers, backend engineers |
| Status | Draft v1.0 (scoring model v1.0.0) |
| Version | 1.0.0 |
| Owner | Framework owner |
| Dependencies | 20 (engine), 22 (confidence), 24 (applicability), 26 (evidence taxonomy) |
| Source references | CRF §5 (category definitions), Appendix A (controls+weights), Appendix B (evidence rules) |
| Assumptions | Freshness values extend ASM-07 conservatively where CRF is qualitative — marked ◆ |
| Open questions | OQ-15 |
| Approval required | Framework owner + R-SA (model publish) |
| Last updated | 2026-07-16 |

## 1. Storage model

Controls are **versioned configuration** (rows in the scoring-model bundle, 20 §8), not code:

```
Control {id, dimension, name, weight (decimal), definition, purpose,
 maturity_anchors[5], evidence_examples[EV-refs], applicability_tags[],
 module_tags[], reviewer_role, freshness_days, red_flag_triggers[],
 materiality_note, i18n keys}
```

Weight edits = MAJOR model change (20 §10). Applicability/N-A handling in 24. Historical versions retained; assessments pin the version used.

## 2. Generic maturity anchors (CRF §4.1) — specialized per control in config

| Level | Anchor | Evidence pattern |
|---|---|---|
| 0 Absent | No process/document/owner or contradicted | Missing/contradicted |
| 1 Informal | Ad hoc, incomplete, stale, person-dependent | Self-declaration, unreconciled spreadsheets, unsigned drafts (≈ tier E0–E1) |
| 2 Documented | Exists on paper; inconsistently implemented/partially evidenced | Approved procedures, partial records |
| 3 Implemented | Operates consistently; current, reconcilable evidence | Regular reporting, approvals, source-system exports (≈ E2) |
| 4 Institutional | Monitored, tested, independently verified, personnel-resilient | Audit/review, registry verification, independent appraisal, tested BCP (≈ E3) |

Rule: **level 4 requires at least one E3-tier evidence item** for the control (or E2 registry/source verification where E3 is not meaningful); level ≥3 requires ≥E2 on material fields. Questionnaire-only support caps a control at level 1 (P5).

## 3. Control registry (weights per CRF Appendix A; Σ = 100)

### A. Financial Information Quality — 15
| ID | Control | W | Assessment focus | Core evidence (26) | Freshness◆ | Red-flag triggers |
|---|---|---|---|---|---|---|
| A1 | Historical financials & accounting basis | 4 | ≥24–36m statements, consistent basis, notes, accountant/auditor involvement | EV-FIN-01/02 | statements ≤ FY+9m; YTD ≤90d | basis changes w/o explanation; missing years |
| A2 | Management reporting cadence & close | 3 | Monthly/quarterly P&L, BS, CF; close calendar; variance notes | EV-FIN-03 | ≤60d | no management accounts; >45d close |
| A3 | Reconciliation to bank/tax/ledgers | 4 | Statements reconcile to GL, bank, tax, AR/AP, debt | EV-FIN-04, EV-BNK-01, EV-TAX-01 | ≤90d | material unreconciled deltas → CAP-01 |
| A4 | Forecast integration & assumptions | 2 | Integrated 3-statement forecast, base/downside, actual-vs-budget | EV-FIN-06 | ≤90d | hockey-stick w/o basis |
| A5 | Data lineage & change control | 2 | Source system, owner, extraction date, transformations recorded | EV-FIN-07 | at assessment | manipulated exports |

### B. Financial Capacity & Resilience — 20
| ID | Control | W | Focus | Evidence | Freshness | Triggers |
|---|---|---|---|---|---|---|
| B1 | Primary repayment / DSCR or equivalent | 5 | Historical+projected CADS; DSCR/FCC where appropriate (metric library CRF §6; startup alternatives 24) | EV-FIN-*, EV-BNK-01 | ≤90d | no credible repayment source → CAP-02 |
| B2 | Liquidity & working capital | 4 | Cash, ratios, WC needs, minimum cash | EV-FIN-01/03, EV-BNK-01 | ≤90d | negative unexplained cash |
| B3 | Profitability/margins & cash conversion | 3 | GM/EBITDA/net, normalization, volatility, CCC | EV-FIN-01/05 | ≤90d | unnormalized one-offs |
| B4 | Leverage, interest & maturity burden | 3 | Debt/EBITDA or alternative, coverage, maturity wall, covenant headroom | EV-DBT-01/02 | ≤90d | undisclosed debt → CAP-03/GATE-04 path |
| B5 | Revenue stability / concentration | 3 | Trend, recurrence, seasonality, churn, top-customer share | EV-REV-01/02, EV-AR-01 | ≤90d | >40% single-customer unmitigated (S3) |
| B6 | Downside resilience | 2 | Stress cases: revenue, margin, FX, rates, delays | EV-FIN-06 | ≤90d | no downside case for module |

### C. Management & Governance — 12
| ID | Control | W | Focus | Evidence | Freshness | Triggers |
|---|---|---|---|---|---|---|
| C1 | Leadership capability & integrity | 3 | Track record, domain expertise, capacity | EV-GOV-01 (CVs), EV-CMP-04 (integrity checks) | 12m | adverse-media hits (route R-CR) |
| C2 | Governance & decision rights | 3 | Board/advisory, reserved matters, delegation, minutes | EV-GOV-02/03 | 12m | decisions outside authority |
| C3 | Key-person/succession & organization | 2 | Role clarity, backups, retention, succession, insurance | EV-GOV-04 | 12m | single-person dependency unmitigated |
| C4 | Ownership alignment / related parties | 2 | Cap table clarity, shareholder agreements, related-party terms, disputes | EV-OWN-01/02/04, EV-REL-01 | 6m | ownership disputes (S2) |
| C5 | Budgeting & accountability | 2 | Budget approval, major-contract & borrowing authority, exception escalation | EV-GOV-05 | 12m | — |

### D. Operations, Controls & Technology — 10
| ID | Control | W | Focus | Evidence | Freshness | Triggers |
|---|---|---|---|---|---|---|
| D1 | Core process maturity & KPIs | 2 | Documented processes, KPIs, SLAs | EV-OPS-01 | 12m | — |
| D2 | Internal controls & segregation | 3 | SoD, approval limits, cash/procurement/payroll/inventory/fraud controls | EV-OPS-02 | 12m | fraud-control absence with cash-heavy ops (S3) |
| D3 | Reporting capability | 2 | Timely ops+financial reporting fit for monitoring | EV-FIN-03, EV-OPS-03 | ≤90d | — |
| D4 | Technology / data / cyber controls | 2 | Reliable systems, access, backups, change mgmt | EV-OPS-04 | 12m | breach history undisclosed |
| D5 | Business continuity / insurance | 1 | Dependencies, recovery, insurance, alternatives | EV-INS-01, EV-OPS-05 | 12m | uninsured critical asset (module-relevant) |

### E. Legal & Corporate Readiness — 13
| ID | Control | W | Focus | Evidence | Freshness | Triggers |
|---|---|---|---|---|---|---|
| E1 | Entity, authority & good standing | 3 | Formation, standing, bylaws, registers, signatories, approvals | EV-COR-01/02/03 | standing ≤30d at approval | GATE-01 fail if unverifiable |
| E2 | Ownership / group / UBO structure | 2 | Entity chart, UBO chain, affiliates | EV-OWN-01/03 | ≤90d | GATE-02 enhanced review triggers (CRF §7) |
| E3 | Debt / liens / guarantees completeness | 2 | Full obligations, security interests, negative pledges, contingent | EV-DBT-01/03 (lien searches) | searches ≤30d at approval | undisclosed lien → CAP-03 + S2 |
| E4 | Material contracts | 2 | Customer/supplier/lease/financing/employment/IP/partnership | EV-CON-01 | 6m | change-of-control traps unassessed |
| E5 | Licenses / regulatory standing | 2 | Permits, registrations, inspections, renewals | EV-LIC-01 | ≤90d | expired critical license (S2) |
| E6 | Litigation / tax / labor claims | 1 | Disputes, judgments, quantified exposure | EV-LEG-01, EV-TAX-02 | ≤90d | material undisclosed litigation → CAP-03 |
| E7 | IP ownership / dependencies | 1 | Chain of title, registrations, OSS/license risks | EV-IP-01 | 12m | unowned core IP (startup: S2) |

### F. Compliance, Integrity & ESG/E&S — 10
| ID | Control | W | Focus | Evidence | Freshness | Triggers |
|---|---|---|---|---|---|---|
| F1 | KYC/KYB/UBO verification | 2 | Identity, existence, control verification, refresh | EV-CMP-01 | at assessment + publication (ASM-07) | GATE-01/02 |
| F2 | Sanctions/PEP/adverse-media process | 2 | Risk-based screening, disposition, records | EV-CMP-02 | at assessment + publication | GATE-03 (any potential match → human) |
| F3 | Source of funds / wealth | 1 | Economic origin, bank trail, plausibility | EV-CMP-03 | ≤90d | unexplained source (S2, R-CR) |
| F4 | Anti-corruption / third parties | 2 | Policy, gifts, third-party diligence, government touchpoints | EV-CMP-04 | 12m | success-fee agents on public contracts (S2) |
| F5 | Tax / labor compliance | 1 | Filings, payments, arrears, social security | EV-TAX-01/02, EV-LAB-01 | ≤90d | material arrears → CAP-03 candidate |
| F6 | ESG/E&S materiality & management | 2 | Sector-appropriate E&S risks, permits, community | EV-ESG-01 | 12m | protected-area/resettlement issues (S2, RE modules) |

### G. Capital Request, Structure & Repayment — 14
| ID | Control | W | Focus | Evidence | Freshness | Triggers |
|---|---|---|---|---|---|---|
| G1 | Purpose & amount rationale | 3 | Specific purpose, evidence-based amount, timing | EV-UOP-01 | current request | vague purpose (blocks module) |
| G2 | Sources & uses / borrower contribution | 3 | Complete plan, equity, fees, contingency, draws | EV-UOP-02 | current | uses ≠ amount |
| G3 | Repayment structure & cash-flow alignment | 3 | Amortization/interest/maturity vs cash cycle | EV-UOP-03, EV-FIN-06 | current | misaligned schedule |
| G4 | Downside / exit / refinancing plan | 2 | Stress case, refinance/exit assumptions, corrective actions | EV-FIN-06 | current | balloon w/o takeout logic |
| G5 | Covenant / reporting capacity | 2 | Ability to produce required reporting, comply with controls | EV-FIN-03, EV-OPS-03 | current | — |
| G6 | Stakeholder approvals / consents | 1 | Sponsor/shareholder/guarantor/existing-lender consents | EV-COR-03, EV-DBT-02 | ≤90d | missing existing-lender consent (S2) |

### H. Collateral / Alternative Support & Monitoring — 6
| ID | Control | W | Focus | Evidence | Freshness | Triggers |
|---|---|---|---|---|---|---|
| H1 | Asset/support identification & ownership | 1.5 | Borrower owns/controls pledged assets; registry details | EV-COL-01, EV-PRO-01 | ≤90d | ownership gaps → module cap CAP-04 |
| H2 | Valuation / eligibility quality | 1.5 | Independent/supportable value, aging, exclusions | EV-COL-02 (appraisal) | appraisal ≤12m◆ (RE), ≤6m (equipment/inventory) | stale/self-valuation |
| H3 | Perfection / enforceability readiness | 1.5 | Registry searches, priority, consents, counsel path | EV-COL-03, EV-LEG-02 | searches ≤30d at approval | prior liens undisclosed |
| H4 | Monitoring / control / insurance | 1.0 | Borrowing base, inspections, reporting, triggers, insurance | EV-COL-04, EV-INS-01 | module-specific | uninsured collateral |
| H5 | Alternative support where unsecured | 0.5 (→6.0 when H1–H4 N/A, 24 §3) | Guarantees, sponsor support, reserves, contracts | EV-GTE-01, EV-CON-01 | ≤90d | guarantor capacity unevidenced |

## 4. Materiality (CRF Appendix B)

Each control carries quantitative + qualitative materiality parameters in config, e.g. A3 reconciliation delta material if >2% of revenue or >$25k (lower of); B4/E3 any single obligation >5% of total debt or >$50k; E6 litigation exposure >5% equity or >$100k. Values are v1.0.0 proposals for Phase-0 calibration (OQ-15). Immaterial + disclosed discrepancies do not trigger CAP-01 (CRF §4.3).

## 5. Reviewer ownership

A,B,D,G,H → R-FR; C,E → R-LR; F → R-CR (12 §2). H valuation evidence may require partner input (R-AP/appraiser via 58).

## 6. Red-flag trigger wiring

Triggers listed above instantiate flags per 23 with default severity shown; severity may be raised, never silently lowered (lowering requires override discipline 23 §7).
