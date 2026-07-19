# 17 — Pilot Fee Schedule v1.0 (Template)

| Field | Value |
|---|---|
| Purpose | The concrete fee **numbers** for the Costa Rica pilot — instantiating the taxonomy in `08` and the config schema in `07`. This is a **template**; rates are filled at partner-negotiation time. |
| Audience | Finance, founder, partner lender, compliance, engineering |
| Status | **DRAFT TEMPLATE — rates are placeholders (`[TBD]`).** Do not treat as final. Requires: partner negotiation, interest-cap test (`08` §4), and a transaction-level tax opinion (`03_...` §11) before it is committed as v1.0. |
| Version | 0.1.0 (pre-numbers) |
| Owner | Finance + Founder |
| Dependencies | `08_FEE_POLICY_AND_PRICING_MODEL.md`, `07_...` §4.6 (config schema), `10_...` §5 (commercial terms) |
| Last updated | 2026-07-19 |

---

## 1. How this document works

This schedule holds the **actual pilot numbers**. Everything structural is already locked in `08`; this file only fills in rates, floors, caps, and effective dates once the partner deal is set. **No number below is final** — placeholders are `[TBD]`. Per the governing rule (`08` §1), rates live here (and in the versioned config), **never** hardcoded in the platform or scattered through SOPs.

## 2. Pre-commit checklist (must all be true before rates are locked)

- [ ] Partner commercial terms agreed (`10` §5).
- [ ] Each fee's `deduct_from_proceeds = false` for capitalYA fees (`08` §5).
- [ ] All-in cost (interest + borrower-paid fees) tested against the **current BCCR cap** for the currency and semester (`08` §4).
- [ ] Transaction-level **tax opinion** obtained (VAT/withholding) (`03_...` §11).
- [ ] Consumer disclosure of all fees prepared (`11` §2).
- [ ] Finance + Compliance sign-off.

## 3. Fee schedule (rates deferred)

| Fee code | Charged by | Payer | Method | Base | Rate/Amount | Min | Max | Currency | Earned event | Deduct from proceeds | Counts toward cap |
|---|---|---|---|---|---|---|---|---|---|---|---|
| `CRP_ASSESSMENT_FEE` | CRP | `[borrower/lender]` | flat | n/a | `[TBD $2,500–5,000]` | — | — | `[USD/CRC]` | assessment_complete | false | `[if borrower-paid: yes]` |
| `CAPITALYA_PLATFORM_FEE` | capitalYA | `[lender/borrower]` | `[subscription/per_application]` | n/a or per-app | `[TBD]` | `[TBD]` | `[TBD]` | `[per model]` | false | `[likely no]` |
| `CAPITALYA_SETUP_FEE` | capitalYA | lender | flat | n/a | `[TBD]` | — | — | `[USD]` | integration_complete | false | no |
| `CAPITALYA_SUCCESS_FEE` | capitalYA | `[borrower/lender]` | percentage | funded_principal | `[TBD bps]` | `[TBD]` | `[TBD]` | `[USD/CRC]` | loan_disbursed | false | `[if borrower-paid: yes]` |
| `CAPITALYA_SERVICING_TECH_FEE` | capitalYA | lender | percentage | `[original/outstanding]` principal | `[TBD bps]` | — | — | `[USD/CRC]` | servicing_period | false | no |

Lender's own interest and origination fee are **set and disclosed by the lender** and are **not** in this schedule (`08` §3.4).

## 4. Config instances (fill and version)

Each row above is stored as a config object (`07` §4.6). Example (still zero-rate until committed):

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
  "effective_from": "[pilot start]"
}
```

When rates are agreed: set `rate_basis_points`/amounts, `effective_from`, bump `version`, record approval, and update this schedule to **v1.0.0** with the checklist (§2) complete.

## 5. Interest-cap worksheet (fill per product/currency)

| Item | Value |
|---|---|
| Product | `[e.g., invoice factoring]` |
| Currency / semester | `[CRC or USD / 2026-H2]` |
| Applicable BCCR cap (effective annual) | `[TBD — check BCCR Jan/Jul publication]` |
| Lender nominal + effective rate | `[TBD]` |
| Borrower-paid fees counted | `[sum of CRP + success fee if borrower-paid]` |
| **All-in effective cost** | `[computed]` |
| Within cap? | `[YES/NO — must be YES to launch]` |

## 6. Change control

Rate changes are config version bumps with effective dates and recorded approval (Finance + Compliance); score integrity is never affected by any fee (`05` §3.10). Superseded versions are retained.

> **Reminder:** this file is deliberately number-free until the partner deal, cap test, and tax opinion are done. Filling it in prematurely would hardcode unfinished economics — exactly what `08` forbids.
