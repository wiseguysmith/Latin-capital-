# 05 — MVP Regulatory Boundary Statement

| Field | Value |
|---|---|
| Purpose | The canonical, quotable statement of what capitalYA (and CRP) **is** and **is not** during the Horizon-1 partner-lending MVP. Every other document, contract, screen, and regulator submission cites this one. |
| Audience | Regulators, partner lender, counsel, product/engineering, investors, borrowers |
| Status | Draft v1.0 — the anchor. Requires counsel confirmation of language (`03_...` §17). |
| Version | 1.0.0 |
| Owner | Founder + Compliance lead (joint) |
| Dependencies | Governs by reference: `06`, `07`, `08`, `09`. Derived from `01`, `02`, `03`, CRP `02_PRODUCT_PRINCIPLES_AND_BOUNDARIES.md` |
| Governing constraint | This restates and consolidates controlling sources; it does not create new scope. On conflict, `03_REGULATORY_STRATEGY_COSTA_RICA.md` wins. |
| Last updated | 2026-07-19 |

---

## 1. The canonical statement (quote this verbatim)

> **capitalYA is a financial-technology and workflow platform. CRP is an informational capital-readiness assessment service. Neither lends money, decides credit, sets loan terms, signs loan contracts, or holds, transmits, or settles any borrower or investor funds. In the capitalYA program, a licensed and supervised partner lender is the sole creditor: it performs final underwriting, approves or declines, sets all terms, signs the loan contract, disburses funds through its own regulated accounts, collects repayment, and files all regulatory and AML reports. The capitalYA Capital Readiness Score is an informational assessment of preparedness — not a credit score, probability of default, loan approval, investment recommendation, public credit rating, or guarantee of funding. The partner lender performs its own independent underwriting and is not bound by capitalYA or CRP.**

This paragraph is the single source of truth for the program's regulatory posture. The Spanish-language equivalent must be validated by counsel before any customer- or regulator-facing use (not machine-translated).

## 2. Role summary (one line each)

- **Borrower (SME):** submits documents, consents to sharing, receives status.
- **CRP:** organizes, verifies, scores, flags, explains → produces a signed **Readiness Assessment Package**. Never approves credit.
- **capitalYA:** intake, workflow, portal, non-binding presentation, status display, servicing technology. Never lends, decides, or holds funds.
- **Partner lender (licensed):** underwrites, approves/declines, sets terms, signs, disburses, collects, reports. The **only** party that lends and touches money.

## 3. What capitalYA / CRP will NOT do in the MVP

Consolidated from `03_...` §16 and CRP `02` §2 — this is the enforceable prohibition list:

1. Receive, hold, custody, transmit, or settle borrower or investor **principal or repayments** in any capitalYA/CRP account.
2. **Lend** money or take balance-sheet credit exposure.
3. **Approve or decline credit**, or set final loan terms, rates, or amounts.
4. **Sign** a loan contract or bind any party to a transaction.
5. Represent an internal capitalYA result as an **approval**, or use "approved / pre-approved / guaranteed / investment-grade / credit rating / low-risk / recommended investment."
6. Sell, market, or facilitate **tokens or transferable loan interests to the public**; operate a DEX or secondary market.
7. Write **personal or financial data to any public blockchain**.
8. Clear a **sanctions match**, make a fraud determination, or make a legal determination by automation.
9. Share borrower identity or data room with the lender **without recorded, scoped consent**.
10. Let **payment or fee status** influence a readiness result, flag, gate, or queue position.
11. Pool investor capital or select loans with discretion (no fund-management activity).
12. Continue the lender's regulated activities if the lender loses authorization (a partner failure is a **wind-down**, not a promotion).
13. Present CRP's score as regulatory approval, a public rating, or a substitute for the lender's underwriting.
14. Allow the Cayman foundation to solicit Costa Rican investors or contract with CR retail borrowers.
15. Treat ERC-3643 (or any token standard) as legal compliance in itself.

## 4. What capitalYA / CRP MAY do

- Coordinate introductions, workflows, information, document requests, and approved service providers.
- Provide a **portal** through which the licensed lender's staff perform and record their own decisions.
- Present a consented readiness package to the lender as **non-binding** decision-support input.
- Validate completeness and integrity of a file against the lender's own intake checklist (no credit judgment).
- Provide servicing **technology** where servicing functions are contractually delegated.
- Display loan status to the borrower sourced from the lender's system of record.
- Charge **fixed, disclosed** technology, assessment, workflow, and documented success fees (per `08`).

## 5. Mandatory limitation statement (travels with every readiness score)

Wherever a Capital Readiness Score appears — score screen, report, exported package, lender handoff — this must appear (EN; counsel-validated ES required for production), per CRP `02` §3:

> The Capital Readiness Score measures how prepared this business is for capital review. It is not a credit score, a probability of default, a loan approval, an investment recommendation, or a guarantee of funding. Capital providers must perform their own independent underwriting, diligence, and approval process.

## 6. How the boundary is enforced (not just promised)

| Boundary | Enforcement mechanism | Where specified |
|---|---|---|
| No funds custody | capitalYA has **no money-movement rails**; all disbursement/repayment runs through lender/PSP accounts | `06` settlement model |
| No credit decision | Legally-meaningful states (`approved/declined/disbursed/…`) are **emittable only by the lender** in the state machine | `06` state machine |
| No capitalYA credit logic (MVP) | capitalYA produces a **validation/completeness** result with `final_credit_decision: null`, never a score/recommendation | `07` |
| Score ≠ rating/approval | Limitation statement is a **non-removable** field of the assessment package | `07` schema; CRP `02` §3 |
| No prohibited terms | Copy-lint of prohibited terms in CI | CRP `73` §6 |
| Fees can't disguise interest | All-in cost tested against BCCR caps; fees are for services | `08` interest-cap rule |
| Consent-gated sharing | Consent ledger; lender sees only consented scope | `02` §5; CRP consent ledger |

## 7. Audiences and uses

- **Regulators (CIF/SUGEF/SUGEVAL):** §1 + §3 are the core of the engagement package (`09`).
- **Partner lender agreement:** §1–§4 become the recitals and the responsibility split (`06`, `04`).
- **Product/engineering:** §3 is a hard build boundary; §6 lists the mechanisms to implement.
- **Borrower & consumer copy:** §1 + §5 framing; §3.5 prohibited terms.
- **Investors / diligence:** §1 sets the honest near-term perimeter (Horizon 1); tokenization is future-phase.

## 8. Change control

Changes to this statement require **Founder + counsel** sign-off (CRP `02` §5 decision rights). Any product, contract, or submission that implies conduct beyond §4 or inside §3 is out of compliance and must be corrected against this document before release.
