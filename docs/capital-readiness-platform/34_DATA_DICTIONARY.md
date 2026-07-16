# 34 — Data Dictionary

| Field | Value |
|---|---|
| Purpose | Field-level definitions for shared/critical attributes: types, enums, derivation rules, sensitivity, and canonical metric definitions |
| Audience | Backend/data engineers, analysts, QA |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Backend lead + Framework owner (metric definitions) |
| Dependencies | 33 (entities), 32 (schemas), 44 (classes) |
| Source references | CRF §6 (metric library), §6.1; Appendix A/B |
| Assumptions | ASM-04 (currencies) |
| Open questions | OQ-15 |
| Approval required | Backend lead |
| Last updated | 2026-07-16 |

## 1. Canonical enums

| Enum | Values |
|---|---|
| org_type | borrower, internal, capital_provider, partner, auditor |
| role | R-PA,R-BA,R-BM,R-IA,R-AR,R-FR,R-LR,R-CR,R-SA,R-CPA,R-CPN,R-LP,R-CP,R-AP,R-AUD,R-SUP |
| applicant_state / assessment_state / document_state / opportunity_state | per 14 §2–5 (exact strings) |
| dimension | A,B,C,D,E,F,G,H |
| maturity | 0,1,2,3,4 (int) — NA represented by na_reason_code ≠ null |
| na_reason_code | NA-STRUCT, NA-TYPE, NA-STAGE, NA-JUR, NA-REQ |
| band | institutional_ready, capital_ready, conditionally_ready, developing, not_ready |
| confidence_grade | A,B,C,D |
| evidence_tier | E0,E1,E2,E3 |
| external_risk_level | 1..5 |
| gate_id | GATE-01..GATE-07 |
| cap_id | CAP-00..CAP-06 |
| flag_severity | S1,S2,S3,S4 |
| flag_effect | gate_link, cap_link, condition, warning |
| privacy_class | P1,P2,P3,P4 |
| defect_code | DEF-01..DEF-10 |
| purpose_module | M-WC, M-EXP, M-EQUIP, M-CONST, M-ACQ, M-REFI, M-VD, M-RBF, M-BRIDGE-RE, M-ABL, M-CONTRACT |
| business_type | BT-SME, BT-STARTUP, BT-RE-DEV, BT-RE-OP, BT-ASSET, BT-AR, BT-PROJECT |
| currency (MVP) | USD, CRC (overlay-extensible) |
| consent_scope | processing, screening, sharing:<provider_org_id|provider_type>, retention |
| outcome | intro_made, term_sheet, funded_external, declined_external, withdrawn, expired (37 taxonomy) |

## 2. Critical field definitions (selection; full listing lives with 33 entity specs)

| Field | Type | Definition / rule | Sensitivity |
|---|---|---|---|
| organization.registry_id | string | CR: cédula jurídica (format 3-101-XXXXXX); overlay-defined validators per country | P2 |
| party.national_id | string | person identifier (CR: cédula/DIMEX/passport) | P4 |
| ownership_edge.pct | decimal(7,4) | direct ownership percentage 0–100 | P4 (person-linked) |
| ubo.aggregate_pct | decimal(7,4) | computed transitive ownership: Σ over all paths Π(edge pct) | P4 |
| ubo.control_basis | enum | ownership, voting, board_control, other_control (per FATF-style standards; overlay threshold default 25% — GATE-02) | P4 |
| capital_request.amount | money | requested principal; soft range $500k–$2M (10 §5) | P3 |
| capital_request.repayment_source | struct | {type: operating_cash_flow|contracted_revenue|asset_sale|refinance|guarantor|mixed, narrative, evidence_refs} — required (CAP-02) | P3 |
| control_assessment.maturity | int 0–4 | per anchors 21 §2; evidence_refs required unless 0-with-absence-note | P2 |
| assessment_result.system_total / published_total | decimal(5,2) → displayed int | 20 §4; published differs only via approved overrides | P3 |
| assessment_result.input_hash | sha256 | hash of canonicalized AssessmentInput for reproducibility (NFR-15) | P1 |
| evidence_field.value | typed json | authoritative value post-verification; provenance mandatory | inherits doc class |
| consent.text_version | string | exact versioned consent text shown (counsel-validated, OQ-14) | P2 |
| audit_event.chain_hash | sha256 | H(event_n) = sha256(payload_n ‖ H(event_{n−1})) per 36 | P1 |
| document.max_age_days | int | from 26 registry; staleness job input (FR-DOC-06) | — |
| flag.visibility | struct | {borrower: full|summary|hidden, provider: disclosed|hidden, restricted: bool} per 23 §6 | P2 |
| override.expiry | date | ≤ assessment expiry (ASM-09) | P2 |

## 3. Canonical metric derivations (deterministic; CRF §6 definitions — cautions apply, no universal thresholds)

| Metric | Formula (canonical fields §32.3) | Notes |
|---|---|---|
| DSCR | cash available for debt service ÷ scheduled P+I | CADS = operating cash flow ± normalization per documented definition; test historical/projected/downside (B1) |
| Fixed-charge coverage | (earnings/CF available) ÷ (interest + principal + leases + fixed) | material leases |
| Current / quick ratio | CA÷CL; (cash+securities+eligible AR)÷CL | AR quality via EV-AR-01 |
| EBITDA (normalized) | net income + interest + tax + D&A ± normalization_items | normalization items itemized (32 §3) |
| Debt/EBITDA | (gross or net, labeled) ÷ normalized EBITDA | suppressed when EBITDA ≤ 0 (BT-STARTUP substitution, 24 §5) |
| Interest coverage | EBIT (or EBITDA, labeled) ÷ cash interest | label the numerator |
| Burn rate / runway | avg monthly net cash consumed; unrestricted cash ÷ burn | startups (B1 substitute) |
| Working capital / CCC | CA−CL; DIO+DSO−DPO | ties to M-WC sizing |
| Gross/EBITDA/net margin | per CRF §6 | trend + volatility stored |
| Customer concentration | top1/top5/top10 share of revenue or AR | B5 |
| Receivables dilution | (credits+returns+disputes+offsets+write-offs) ÷ gross AR | M-ABL |
| LTV | loan balance ÷ supportable collateral value | recovery metric, never substitutes cash-flow analysis (CRF §6) |

Every derived metric stores: inputs used, formula version, computation timestamp. Benchmarks/thresholds are **not** hardcoded; provider-configurable views are post-MVP (CRF §6 "no universal threshold rule").

## 4. Sensitivity & lineage

Every field in 33 entities carries privacy_class (44 §3) and, for evidence-derived fields, full lineage to source (30 §6). Analytics (37) consumes only P1/P2 aggregates or de-identified extracts.
