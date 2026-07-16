# 32 — Extraction Schemas

| Field | Value |
|---|---|
| Purpose | Versioned structured-output contracts between AI extraction and backend, per document type; canonical financial data model |
| Audience | AI engineers, backend engineers |
| Status | Draft v1.0 (schema bundle v1.0.0) |
| Version | 1.0.0 |
| Owner | AI lead + Backend lead |
| Dependencies | 26 (evidence registry), 30 (pipeline), 34 (data dictionary) |
| Source references | CRF §6.1 (required analyses/fields), §16 layers 2–3 |
| Assumptions | JSON Schema draft 2020-12; all schemas share the base envelope |
| Open questions | — |
| Approval required | AI + backend leads |
| Last updated | 2026-07-16 |

## 1. Base envelope (all document types)

```json
{
  "$id": "crf/extraction/base-1.0",
  "type": "object",
  "required": ["doc_type", "doc_type_confidence", "language", "entity_names",
               "period", "fields", "quality"],
  "properties": {
    "doc_type": {"enum": ["<EV ids from 26>"]},
    "doc_type_confidence": {"type": "number", "minimum": 0, "maximum": 1},
    "language": {"enum": ["es", "en", "other"]},
    "entity_names": {"type": "array", "items": {"$ref": "#/defs/entity_mention"}},
    "period": {"start": "date|null", "end": "date|null", "as_of": "date|null"},
    "signatures": {"present": "bool", "signatories": ["entity_mention"], "notarized": "bool|null"},
    "fields": {"type": "object"},          // per-type schema below
    "quality": {"complete_pages": "bool", "legibility": "0..1",
                "defects": [{"code": "DEF-xx", "note": "string", "page": "int"}]},
    "provenance_map": {"<field_path>": {"page": "int", "bbox": [4 numbers], "confidence": "0..1"}}
  }
}
```

Rules: every leaf in `fields` MUST have a `provenance_map` entry; extractor returns `null` + `defects` rather than guessing; amounts always `{value, currency, scale}`; dates ISO-8601; percentages 0–100 decimals.

## 2. Per-type field schemas (v1.0 core set — one per high-volume evidence type; others follow the pattern)

**EV-FIN-01 Annual financial statements** → canonical statement model §3 + `{accounting_basis: IFRS|local_gaap|other, auditor_involvement: audited|reviewed|compiled|none, auditor_name, opinion_type, comparative_periods: bool, notes_present: bool}`.

**EV-BNK-01 Bank statement** → `{bank_name, account_number_masked, account_holder, currency, period, opening_balance, closing_balance, total_inflows, total_outflows, monthly_aggregates: [{month, inflows, outflows, min_balance, end_balance}], nsf_or_reversal_count}`.

**EV-TAX-01 Tax filing** → `{authority, tax_type, period, taxable_base, tax_assessed, tax_paid, filing_date, payment_status, arrears_flag}`.

**EV-DBT-01 Debt schedule** (CRF §6.1 fields) → `{items: [{lender, original_amount, balance, currency, rate, rate_type, maturity_date, amortization, collateral_desc, covenants_summary, arrears_status, guarantee_flag}] , total_debt}`.

**EV-DBT-02 Loan agreement** → `{parties, principal, currency, rate, maturity, amortization_type, security_interests: [..], covenants: [..], negative_pledge: bool, change_of_control: bool, guarantees: [..], defaults_remedies_present: bool}`.

**EV-COR-01/02 Corporate/registry docs** → `{entity_legal_name, registry_id (CR: cédula jurídica), entity_type, formation_date, status, registered_capital, legal_reps: [{name, id, powers}], registry_issue_date}`.

**EV-OWN-01 Ownership/UBO chart** → `{nodes: [{id, kind: person|entity, name, id_doc?, jurisdiction?}], edges: [{from, to, pct, instrument}], declared_ubos: [{person, aggregate_pct, control_basis}]}` (feeds ownership graph ENT-06/07; cycle/coverage validation deterministic).

**EV-AR-01 AR aging** → `{as_of, buckets: {current, d30, d60, d90, d90_plus}, total, top_debtors: [{name, amount, pct}], disputed_amount}` (AP mirror for EV-AP-01).

**EV-CON-01 Material contract** → `{parties, effective_date, term_end, auto_renew, annual_value, currency, termination_rights_summary, assignment_restriction: bool, change_of_control: bool, exclusivity: bool, governing_law}`.

**EV-COL-02 Appraisal** → `{appraiser, license_id?, asset_desc, approach: [..], value, currency, effective_date, assumptions_flags, conditions}`.

**EV-PRO-01 Title study / property registry** → `{property_id (CR: folio real), owner, area, location, encumbrances: [{type, holder, amount?, rank}], annotations, issue_date}`.

**EV-LIC-01 License/permit** → `{issuer, scope, number, issue_date, expiry_date, conditions, status}`.

**EV-INS-01 Insurance certificate** → `{insurer, policy_no, coverage_type, limits, deductible, insured, beneficiary/loss_payee, period}`.

**EV-UOP-01/02 Use of proceeds / sources & uses** → `{uses: [{item, amount}], sources: [{source, amount, type: debt|equity|internal}], contingency_pct, draw_schedule?: [{milestone, amount, date}]}`.

**EV-FIN-06 Forecast** → `{horizon_months, cases: [base|downside|upside → {monthly: [{month, revenue, ebitda, cash_flow, cash_balance}]}], key_assumptions: [{name, value, basis}]}`.

**EV-REV-03 MRR/ARR & cohorts (startup)** → `{mrr_series: [{month, mrr, new, expansion, churned}], arr, net_revenue_retention, logo_churn_pct, cohort_table_ref}`.

## 3. Canonical financial data model (normalizer target; CRF §16 layer 3)

`FinancialPeriod {org, period, basis, currency, statements: {income: {revenue, cogs, gross_profit, opex, ebitda, depreciation_amortization, ebit, interest_expense, tax, net_income, normalization_items: [{desc, amount, category: owner_comp|one_off|related_party}]}, balance: {cash, receivables, inventory, other_current, fixed_assets, other_assets, payables, short_term_debt, other_current_liab, long_term_debt, other_liab, equity}, cash_flow: {operating, investing, financing, net}}, derived: {dscr?, current_ratio, quick_ratio, gross_margin, ebitda_margin, net_margin, debt_ebitda?, debt_equity, interest_coverage?, working_capital, ccc_days?, burn_rate?, runway_months?}}` — derived metrics computed **deterministically** by the metrics service using CRF §6 definitions (numerators/denominators documented in 34); AI proposes only chart-of-accounts mappings, humans confirm unmapped lines.

## 4. Versioning & evolution

Schemas semver-versioned as a bundle; additive = MINOR, breaking = MAJOR with dual-write window; every stored extraction records `schema_version` + `bundle_version` (30 §9). Adding a new evidence type requires: 26 registry row + schema + golden fixtures (31 §8) + reconciler rules where cross-checkable.

## 5. Validation rules beyond schema (deterministic post-validators)

Balance sheet balances (assets = liabilities+equity ± tolerance); period continuity; aging buckets sum to total; UoP uses sum to request amount ±1%; ownership percentages 0–100 and sum checks; date sanity (no future statements); currency whitelist (USD, CRC + overlay additions). Failures → defect DEF-07/09 or exception, never silent acceptance.
