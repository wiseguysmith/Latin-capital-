# 18 — Partner Commercial Schedule (Template)

| Field | Value |
|---|---|
| Purpose | The negotiated **lender-side economics** of the capitalYA ↔ partner-lender relationship — the commercial annex to the definitive agreement (`10`). Companion to the borrower-facing Pilot Fee Schedule (`17`). |
| Audience | Founder, finance, partner lender, counsel |
| Status | **DRAFT TEMPLATE — all values are placeholders (`[TBD]`).** Filled during partner negotiation; executed as a schedule/annex to the definitive agreement, not as a standalone contract. |
| Version | 0.1.0 (pre-numbers) |
| Owner | Founder + Finance |
| Dependencies | `10_PARTNER_LENDER_AGREEMENT_TERM_SHEET.md` §5, `08_FEE_POLICY_AND_PRICING_MODEL.md`, `17_PILOT_FEE_SCHEDULE_V1_TEMPLATE.md`, `07_...` §4.6 (config schema); `03_...` §4.4 (fee-risk posture), §11 (tax) |
| Last updated | 2026-07-19 |

---

## 1. How `17` and `18` divide the world

| | `17` Pilot Fee Schedule | `18` Partner Commercial Schedule (this doc) |
|---|---|---|
| Answers | What each fee **is** and what the borrower/lender is charged | What capitalYA and the lender have **agreed between themselves** |
| Audience | Operational config + disclosure basis | Contract annex between the two companies |
| Contains | Fee config instances, cap worksheet | Negotiated rates, payment terms, volume terms, remedies |
| Becomes final | After cap test + tax opinion | On execution of the definitive agreement |

Rates must **match** between the two documents where they overlap (e.g., the success fee); `17` is the config source of truth, this schedule is the contractual record of the same numbers.

## 2. Boundary constraints on the economics (non-negotiable)

Whatever is negotiated, the deal must stay inside the safe-fee perimeter (`03_...` §4.4; `08`):

- Fees are **fixed and disclosed** — subscription, per-application, setup, workflow, documented success fee, servicing-technology fee.
- **Prohibited:** percentage of loan interest, first-loss return, borrower-vs-investor spread, guaranteed minimum yield, discretionary participation in principal, payment conditioned on investor performance.
- **No capitalYA fee deducted from loan proceeds** (`08` §5).
- The lender's interest and origination fee are the **lender's own** revenue, set and disclosed by the lender (`08` §3.4).
- No provision may give capitalYA creditor powers (approval rights, term-setting, funding obligations, repurchase guarantees) — that would undo the `05` boundary.

## 3. Commercial terms (fill at negotiation)

### 3.1 Fees payable by the Lender to capitalYA

| Item | Structure | Amount/Rate | Invoicing | Payment terms |
|---|---|---|---|---|
| Platform subscription | `[monthly/quarterly]` | `[TBD]` | `[monthly in advance]` | `[net 15/30]` |
| Integration/setup fee | one-time | `[TBD]` | on `integration_complete` | `[TBD]` |
| Per-application technology fee | per application reaching `ready_for_lender_review` | `[TBD]` | `[monthly arrears]` | `[TBD]` |
| Origination-support / workflow fee | `[per funded loan / monthly]` | `[TBD]` | `[TBD]` | `[TBD]` |
| Servicing-technology fee | `[bps on original/outstanding principal]` | `[TBD]` | `[monthly arrears]` | `[TBD]` |

### 3.2 Fees payable by the Borrower (recorded here for consistency with `17`)

| Item | Payee | Amount/Rate | Collected how |
|---|---|---|---|
| CRP assessment fee | CRP | `[TBD flat]` | invoiced by CRP — **never from loan proceeds** |
| capitalYA success fee | capitalYA | `[TBD bps of funded_principal]` | invoiced separately — **never from loan proceeds** |

### 3.3 Lender's own charges (informational — lender-set)

| Item | Notes |
|---|---|
| Interest rate(s) | Lender-set per product/tier; all-in cost subject to BCCR cap test (`17` §5) |
| Lender origination fee | Lender-set and lender-disclosed |
| Late charges / collection costs | Lender-set, disclosed per `11` |

## 4. Volume, exclusivity & pilot commitments

| Term | Value |
|---|---|
| Pilot volume cap | `[N borrowers / $X aggregate]` (mirrors `10` §3) |
| Minimum volume commitment | `[none for pilot / TBD]` |
| Exclusivity | `[none / limited scope+duration]` — mark binding if agreed |
| Referral obligations | `[TBD]` |
| Pricing review trigger | `[end of pilot / volume threshold / semester cap republication]` |

## 5. Invoicing, taxes & currency

- Invoicing entity: `[capitalYA CR S.A. / CRP entity]`; currency `[USD/CRC]`; FX reference `[TBD]`.
- **Taxes:** amounts exclusive/inclusive of VAT `[TBD]`; withholding treatment per the transaction-level tax opinion (`03_...` §11) — obtain **before** execution.
- Late-payment interest on inter-company invoices: `[TBD]` (commercial only; unrelated to loan economics).

## 6. Remedies, credits & adjustments

| Event | Remedy |
|---|---|
| capitalYA platform SLA miss (`06` §10) | `[service credits: TBD]` |
| Lender SLA miss (review/decision timelines) | `[escalation per governance; no fee penalty unless agreed]` |
| Sustained platform unavailability | `[credit schedule / termination right per 10 §11]` |
| Fee disputes between the parties | `[governance contacts → escalation ladder per 15 §4]` |

## 7. Term & change control

- Effective: on execution of the definitive agreement; expires/renews with it (`10` §11).
- Rate changes: written amendment to this schedule **+** config version bump in `17`/`07` with effective dates — the two must never diverge.
- Each executed version is retained; the platform config references the executed version ID.

## 8. Pre-execution checklist

- [ ] All `[TBD]` values filled and internally approved (Finance + Founder).
- [ ] Perimeter check against §2 (no prohibited economics) — Compliance sign-off.
- [ ] Rates mirrored into `17` and the versioned fee config (`07` §4.6).
- [ ] Interest-cap worksheet (`17` §5) passes for every product/currency.
- [ ] Tax opinion covers every fee row (`03_...` §11).
- [ ] Counsel confirms the schedule matches the definitive agreement's defined terms.

> **Reminder:** this schedule records a negotiated deal — it cannot be filled in unilaterally. Until a partner signs, every number stays `[TBD]`.
