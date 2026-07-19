# 10 — Partner Lender Agreement: Term Sheet / Heads of Terms

| Field | Value |
|---|---|
| Purpose | Capture the commercial terms and the responsibility/liability allocation for the capitalYA ↔ partner-lender relationship, as the basis for a counsel-drafted definitive agreement |
| Audience | Founder, counsel, the prospective partner lender, compliance, finance |
| Status | **DRAFT TERM SHEET — non-binding.** Not a contract. Requires Costa Rican banking/commercial counsel to draft the definitive agreement. Placeholders in `[BRACKETS]`. |
| Version | 1.0.0 |
| Owner | Founder + counsel |
| Dependencies | `04_ENTITY_AND_PARTNER_STRUCTURE.md` §4–§6, `05_MVP_REGULATORY_BOUNDARY_STATEMENT.md`, `02_INTEGRATED_ARCHITECTURE.md` §7, `06_...`, `08_...`; `03_...` §4 |
| Last updated | 2026-07-19 |

---

## 0. How to use this

This is a **heads-of-terms** to align capitalYA and the lender before counsel drafts the definitive agreement. It is **non-binding** except any clause the parties expressly mark binding (e.g., confidentiality, exclusivity). Nothing here is a legal opinion; the definitive agreement governs.

## 1. Parties

- **capitalYA:** `[capitalYA CR S.A.]`, a Costa Rican technology and assessment platform (and, as applicable, `[CRP entity]` for assessment services).
- **Lender:** `[PARTNER LENDER]`, a `[licensed/supervised]` Costa Rican `[bank / financiera / supervised lender]`, `[SUGEF registration/authorization ref]`.

## 2. Recitals (the boundary, restated)

Per `05`: **the Lender is the sole originator and creditor.** capitalYA provides technology, borrower intake, document management, origination-support workflow, non-binding matching, and servicing technology. capitalYA **does not** lend, decide credit, set terms, sign loan contracts, or hold/transmit funds. CRP provides an **informational readiness assessment**, not a credit rating, approval, or guarantee.

## 3. Scope of the pilot

- Product(s): `[SME invoice factoring / working-capital loans]`.
- Structure: **single lender, portal-first** (`06`).
- Volume: closed pilot, limited to `[N borrowers / $X aggregate]`.
- Term: `[pilot start]` – `[pilot end]`, renewable by mutual agreement.
- Territory: Costa Rica.

## 4. Roles & responsibility matrix (the core of the deal)

| Function | capitalYA / CRP | Lender |
|---|---|---|
| Borrower intake, document management | ✅ | — |
| Readiness assessment (CRP) | ✅ (informational) | consumes as input |
| Completeness validation vs. lender checklist | ✅ | defines checklist |
| **Final underwriting, approve/decline** | ❌ | ✅ |
| **Interest rate & loan terms** | ❌ | ✅ |
| **Borrower loan contract** | ❌ | ✅ (owns) |
| **Disbursement** | ❌ | ✅ (own/PSP accounts) |
| **Collections & enforcement** | tech only | ✅ |
| Consumer disclosures | supports | ✅ (responsible) |
| **KYC of record & AML reporting** | complementary/preliminary | ✅ (of record) |
| Credit-file access & reporting (CIC/CICOC) | ❌ | ✅ |
| Regulatory reporting | ❌ | ✅ |
| Loan-loss accounting | ❌ | ✅ |
| Restructuring & default decisions | ❌ | ✅ |
| Servicing technology | ✅ | ✅ (legal servicer unless delegated) |

## 5. Commercial terms (numbers deferred to `[Pilot Fee Schedule / 17]`)

- **capitalYA platform/workflow fee:** `[structure: subscription / per-application / setup]`, `[amount TBD]`.
- **capitalYA success fee:** `[% of funded_principal TBD]`, earned on `loan_disbursed`, disclosed, **separated from interest income**, **never deducted from proceeds** (`08` §5).
- **CRP assessment fee:** flat `[$2,500–$5,000 TBD]`, paid by `[borrower/lender]`, earned on assessment completion, not contingent on financing.
- **Servicing-technology fee:** `[base: original/outstanding principal TBD]`.
- **Lender's own charges** (interest, origination fee): set and disclosed by the Lender; **not** capitalYA revenue.
- **Interest-cap compliance:** all-in cost tested against BCCR caps; fees not used to evade caps (`08` §4).
- **No capitalYA guarantee** of principal or yield; no first-loss, no spread, no minimum yield (`03_...` §4.4).

## 6. Required definitive-agreement clauses (from `04` §4)

capitalYA's inability to bind the Lender; final-decision authority reserved to Lender personnel; Lender owns credit policy; fee schedule separated from interest; Lender representations of authorization/good standing; AML/KYC/sanctions/reporting responsibility matrix; borrower-consent & data-sharing; loan-document ownership; audit & regulator-access rights; cybersecurity & incident notification; servicing standards + backup servicer; **no commingling of funds**; records export & transition assistance; change-of-control & change-in-law; mutual indemnification for regulatory breaches; **tokenization restrictions requiring prior legal approval**; wind-down/loan-transfer; business-continuity/DR.

## 7. Funds flow (binding principle)

The Lender disburses and receives all loan funds through its own or its regulated processor's accounts. **capitalYA never holds principal or repayments.** Daily reconciliation files to capitalYA for status display (`06` §8). capitalYA is **not** the legal payment ledger.

## 8. AML/CFT allocation

Lender is the **primary regulated AML reporting entity**; capitalYA/CRP conduct **complementary** screening and escalate to the Lender's MLRO **without tipping off** (`02` §7). The definitive agreement fixes: who files, who investigates, who retains the file, who responds to regulator requests, and how capitalYA transmits observations (`03_...` §8.1).

## 9. Data protection

Controller/processor roles defined per Ley 8968; cross-border/US-cloud consent; PRODHAB registration analysis; encryption; retention/deletion; DSAR handling (`13`, `03_...` §9.1).

## 10. Lender failure / wind-down (binding intent)

On loss of authorization or failure, the definitive agreement auto-triggers the wind-down protocol in `16` / `04` §5: stop new originations and any token issuance; move collections to an approved account; appoint backup servicer; deliver borrower/loan files; preserve payment instructions; notify investors/regulators. **capitalYA does not assume the Lender's regulated functions.**

## 11. Term, termination, exclusivity, confidentiality

- Term & renewal: `[…]`. Termination for cause/convenience: `[notice periods]`.
- Exclusivity: `[none / limited]` — **mark binding if agreed.**
- Confidentiality & IP: capitalYA retains platform IP; Lender retains its credit policy IP — **mark binding.**
- Survival: confidentiality, data-protection, wind-down, indemnity survive termination.

## 12. Conditions precedent to the pilot

Counsel's banking-law opinion (`03_...` §17 Q1–Q3); Lender authorization confirmed; responsibility matrix signed; funds-flow confirmed with no capitalYA custody; tokenization disabled; consent/privacy docs (`13`) finalized; cyber/E&O bound.

## 13. Open items → definitive agreement / counsel

`[Governing law & dispute forum]`, `[SLA credits/remedies]`, `[liability caps]`, `[reconciliation file format]`, `[backup-servicer identity]`, `[data-controller determination per data set]`.

> **Next step:** hand this term sheet to Costa Rican banking/commercial counsel to produce the definitive agreement. Do not operate under this document alone.
