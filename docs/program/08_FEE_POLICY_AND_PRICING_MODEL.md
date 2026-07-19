# 08 — Fee Policy, Catalogue & Configurable Pricing Model

| Field | Value |
|---|---|
| Purpose | Lock the fee **taxonomy, calculation logic, governance, and configuration model** now; defer exact commercial percentages to the pilot schedules and contracts. |
| Audience | Finance, product, compliance, legal, partner lender, engineering |
| Status | Draft v1.0 — numbers deferred to a Tier-2 Pilot Fee Schedule and the partner commercial schedule |
| Version | 1.0.0 |
| Owner | Founder + Finance + Compliance |
| Dependencies | `05_MVP_REGULATORY_BOUNDARY_STATEMENT.md`, `04_ENTITY_AND_PARTNER_STRUCTURE.md`, `07_...` (canonical fee-config schema); `03_...` §4.4 (fee risk), §10 (interest caps), §11 (tax) |
| Governing rule | Lock responsibilities, formulas, and controls now; lock exact percentages in pilot schedules and contracts. Never hardcode unfinished economics into the platform or policies. |
| Last updated | 2026-07-19 |

---

## 1. The principle (why numbers are deferred)

Exact percentages do **not** belong in the architecture, operating model, or compliance policies. What must be locked now is the **fee taxonomy and calculation logic**. Exact numbers are finalized only in: the lender partner agreement, the borrower fee schedule, the pilot commercial schedule, investor documents (once investment products exist), and a **version-controlled pricing configuration**. Percentages are never hardcoded into the platform or repeated across SOPs — that turns documentation into expensive fiction.

## 2. Fee Policy — governance rules (permanent)

For **every** fee, the following attributes are documented and version-controlled (this is the required decision set, decided now even where the number is deferred):

| Attribute | Meaning |
|---|---|
| Who charges it | The entity billing the fee |
| Who pays it | Borrower / lender / investor |
| When earned | The `earned_event` (e.g., assessment complete, loan disbursed) |
| Refundable? | Conditions and amounts |
| Calculation base | Flat / % of what (`funded_principal`, `outstanding_principal`, per-application, subscription) |
| Minimum / maximum | Floors and caps |
| Currency | USD / CRC |
| Tax treatment | VAT/withholding per `03_...` §11 |
| Deduct from proceeds? | **`false` for all capitalYA fees** (§5) |
| Affects interest cap? | Whether it counts toward the BCCR all-in cap test (§4) |
| Required disclosure | Where/how disclosed to borrower/consumer |
| Accounting treatment | Revenue recognition |
| Exception approval authority | Who may approve a deviation |

### 2.1 Approval authority

Standard fees follow the versioned config. Exceptions require named approval (Finance + Compliance); score integrity is never affected by any fee (CRP `02` P10 / `05` §3.10). No analyst or reviewer is compensated on score uplift or funding outcomes.

## 3. Fee Catalogue (Horizon 1)

Types and formulas; **numbers live in the Pilot Fee Schedule (Tier 2)**, not here.

| Fee | Charged by | Paid by | Base | Earned event | Risk posture (`03_...` §4.4) |
|---|---|---|---|---|---|
| **CRP assessment fee** | CRP | Borrower (or lender, per deal) | **Flat** | Assessment completion | Green/Yellow — flat, **not** contingent on financing |
| **capitalYA platform fee** | capitalYA | Lender (and/or borrower) | Subscription / per-application / setup / workflow | Per model | Green/Yellow — fixed, disclosed |
| **capitalYA success fee** | capitalYA | Borrower or lender (per structure) | % of `funded_principal` (documented introduction) | `loan_disbursed` | Yellow — disclosed, separated from interest |
| **Servicing-technology fee** | capitalYA | Lender | Original or outstanding principal (defined per deal) | Servicing period | Green/Yellow — technology only |
| **Lender origination fee** | **Lender** | Borrower | Lender-defined | Lender-defined | Lender's own charge — **not capitalYA revenue** unless legal/tax structure explicitly permits |

### 3.1 CRP assessment fee — refund rules

Flat fee for document analysis, readiness review, and remediation planning; **earned on assessment completion**, not on financing approval (`03_...` §5.2). Clear refund rules if the applicant withdraws before the assessment is performed. Not contingent on outcome.

### 3.2 capitalYA platform fee — permitted structures

Monthly lender subscription, per-application technology fee, integration/setup fee, workflow-management fee, or successful-funding fee — any combination, all fixed and disclosed.

### 3.3 Servicing fee — required clarifications

The agreement must state: who **legally** services the loan; whether capitalYA provides **technology only**; base = original vs. outstanding principal; treatment of delinquent/defaulted loans; whether third-party collection fees are separate.

### 3.4 Lender origination fee — keep it the lender's

The lender determines and discloses its own lending charges. capitalYA does **not** present the lender's origination fee as capitalYA revenue unless the legal and tax structure explicitly permits it (`03_...` §4.4, §11).

## 4. Interest-cap aggregation rule (Costa Rica)

Costa Rican law imposes maximum annual rates and provides that **fees and commissions cannot be used to evade the caps** (`03_...` §10.1). Therefore:

- The **all-in cost of credit** — lender interest **plus** every fee that functions as a cost of borrowing (CRP assessment fee where borrower-paid, capitalYA success fee, lender origination fee) — must be tested against the applicable BCCR cap for the currency and semester of the contract.
- Each fee carries `counts_toward_interest_cap` in its config (`07` §4.6). Fees that count are summed into the all-in test.
- capitalYA/CRP fees must be **defensibly for services rendered**, not disguised interest. Where a borrower-paid fee could be recharacterized as interest, it is included in the cap test by default.
- The cap check runs before an offer is presented; a configuration or offer that would breach the cap is blocked. Actual caps are checked per currency/semester (BCCR publishes in January and July).

## 5. Funds-flow constraint on fees

- **No capitalYA fee is ever deducted from loan proceeds.** `deduct_from_proceeds` is **forced `false`** for all capitalYA fees — enforced as schema validation (`07` §4.6), not a free toggle. Only the lender touches proceeds (`05`, `06` §8).
- capitalYA fees are billed and collected **separately** from the loan (invoice/subscription/PSP to capitalYA's own operating account), never from principal or repayment flows.
- **Tax:** "not a lender" is a regulatory posture, not a tax exemption — capitalYA/CRP fee income is taxable; obtain a transaction-level tax opinion before setting prices (`03_...` §11).

## 6. Configurable pricing model

Fees are stored as **versioned configuration** using the canonical schema in `07` §4.6. Example (zero-rate placeholder — structure locked, rate deferred):

```json
{
  "fee_code": "CAPITALYA_SUCCESS_FEE",
  "version": "1.0",
  "calculation_method": "percentage",
  "calculation_base": "funded_principal",
  "rate_basis_points": 0,
  "minimum_amount": null,
  "maximum_amount": null,
  "currency": "USD",
  "payer": "borrower",
  "earned_event": "loan_disbursed",
  "deduct_from_proceeds": false,
  "counts_toward_interest_cap": true,
  "effective_from": "2026-09-01"
}
```

- The **zero rate is intentional**: the schema and controls are locked before the commercial rate is finalized in the pilot schedule.
- Rate changes are config version bumps with effective dates, audited; no code change, no edits scattered through SOPs.

## 7. Investor & yield fees (deferred until tokenization)

**Not required for the Q3 partner-lender MVP** — investors and tokenized loan participations are not active (`05` §3.6; `01` Horizon 2). Before any investor capital is accepted, the waterfall must be made exact and there must be **no hidden spread**:

```
Borrower payment
  − taxes
  − bank/payment charges
  − servicing fee
  − trustee/custody expenses
  − reserve contribution
  − permitted recovery expenses
  = distributable amount
```

Then document how the distributable amount divides among: investor interest, investor principal, lender retained interest, capitalYA fees, default reserves, and any junior/first-loss position. This is specified in a separate **Investor Waterfall Specification** when Horizon 2 is authorized (gated by `03_...` §6).

## 8. Documentation structure (what exists where)

| Document | Status | Contents |
|---|---|---|
| **Fee Policy** (this doc §2) | Now | Permanent governance rules |
| **Fee Catalogue** (this doc §3) | Now | Available types and formulas |
| **Configurable pricing model** (this doc §6, schema in `07`) | Now | Versioned config; zero-rate locked |
| **Pilot Fee Schedule v1.0** | Tier 2 | Actual numbers for the pilot |
| **Partner Commercial Schedule** | Tier 2 | Negotiated lender economics |
| **Investor Waterfall Specification** | Deferred | Until tokenization is authorized |

The rule restated: **lock responsibilities, schemas, formulas, and controls now; lock exact commercial percentages in the pilot schedules and contracts.**
