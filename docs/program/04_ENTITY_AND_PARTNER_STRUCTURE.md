# 04 — Entity, Partner & Contract Structure

| Field | Value |
|---|---|
| Purpose | Define the legal entities, their roles, the partner-lender relationship, and the contractual liability split for the capitalYA + CRP program |
| Audience | Founders, legal/compliance, finance, partner-lender counterparts, investors conducting diligence |
| Status | Draft v1.0 — subject to counsel validation (`03_...` §17) |
| Version | 1.0.0 |
| Owner | Founder + external counsel (joint) |
| Dependencies | `01_CAPITALYA_PROGRAM_BRIEF.md`, `02_INTEGRATED_ARCHITECTURE.md`, `03_REGULATORY_STRATEGY_COSTA_RICA.md`; CRP `58_PARTNER_AND_SERVICE_PROVIDER_MODEL.md`, `64_REGULATORY_AND_LEGAL_VALIDATION_CHECKLIST.md` |
| Governing constraint | Nothing here is a legal opinion. Structure is provisional until the 18 counsel questions in `03_...` §17 are answered in writing. |
| Last updated | 2026-07-19 |

---

## 1. Design goal

Structure the program so that **regulated functions live in regulated entities**, and capitalYA/CRP keep a defensible "not a lender, not a custodian, not a securities issuer" posture. The organizing principle from `03_...` §12: separate technology, assessment, licensed origination, and (later) issuance/custody/placement into distinct parties — never let capitalYA become the entire stack.

## 2. Entity map (Horizon 1)

| Entity | Jurisdiction | Role | What it is | What it is NOT |
|---|---|---|---|---|
| **capitalYA CR S.A.** | Costa Rica | Operating company | Technology, borrower intake, document management, origination-support, workflow orchestration, servicing technology, non-binding matching (`03_...` §2) | Not a lender, bank, deposit-taker, investment platform, exchange, fund manager, or guarantor |
| **CRP (assessment)** | Costa Rica (within or alongside capitalYA CR S.A.; see §3) | Assessment entity | Readiness assessment: document completeness, governance/financial evaluation, data-quality flags, risk identification, score, remediation, evidence-backed report (`03_...` §2) | Not a credit-rating agency, underwriter, or approver |
| **Partner lender** | Costa Rica | Licensed / supervised creditor | Final underwriting, approve/decline, terms, borrower contract, disbursement, collections, consumer disclosures, AML reporting, regulatory reporting, loss accounting, restructuring (`03_...` §2) | Not controlled by capitalYA; capitalYA cannot bind it |
| **US parent** (optional) | United States | Holding / IP / fundraising | Cap table, investor entity, group IP holding (structure TBD with counsel) | Not a party to Costa Rican lending |
| **Cayman foundation** | Cayman Islands | Protocol IP / governance (Horizon 2) | May own protocol IP or coordinate governance later (`03_...` §2) | Must **not** contract with CR retail borrowers, solicit CR investors, receive repayments, make credit decisions, control lender funds, or issue CR retail tokens |

**Correction carried from `03_...`:** the Cayman foundation is **not** a "sovereign-immunity" or "regulatory-arbitrage" shield. A foreign entity is still captured by CR offering law when it offers into Costa Rica. The foundation is deferred to Horizon 2 and kept away from all customer-facing CR activity in Horizon 1.

## 3. CRP: separate entity or ring-fenced function?

Open structural question for counsel (`03_...` §17 Q4, §5.1): CRP can operate either as a **distinct legal entity** or as a **contractually ring-fenced service line** inside capitalYA CR S.A. Either way it must:

- Publish its assessment-only scope and the limitation statement (CRP `02` §3).
- Keep methodology governance, version control, and an evidence log per score (`03_...` §5.3).
- Charge a **flat fee** ($2,500–$5,000), never contingent on financing closing (`03_...` §5.2; CRP D-21).
- Avoid issuer-paid, outcome-contingent compensation and any "rating for public solicitation" (`03_...` §5.1).

**Recommendation:** decide entity vs. ring-fence with counsel before the pilot, because it affects PRODHAB registration, liability limitation, and how CRP's fee is invoiced. Until then, treat CRP as a governed, separable service line with its own terms of service.

## 4. Partner-lender relationship — required contract terms

The partner agreement is the single most important legal instrument in Horizon 1. Per `03_...` §4.2 it must include, at minimum:

- Express statement that the **lender is the sole originator and creditor**.
- capitalYA's **inability to bind** the lender; final-decision authority reserved to lender personnel.
- Lender **owns credit policy** and sets all terms.
- **Fee schedule separated from interest income**; no capitalYA guarantee of principal or yield.
- Lender **representations** of authorization and good standing.
- **AML/KYC/sanctions/reporting responsibility matrix** (see `02_...` §7).
- Borrower-consent and **data-sharing** provisions (Ley 8968-compliant).
- **Loan-document ownership** and audit/regulator access rights.
- **Cybersecurity and incident-notification** obligations.
- **Servicing standards + backup servicer**; **no commingling of funds**.
- **Records export + transition assistance**.
- **Change-of-control and change-in-law** clauses.
- **Indemnification** for regulatory breaches caused by each party.
- **Tokenization restrictions** requiring prior legal approval.
- **Wind-down / loan-transfer** and **business-continuity / DR** obligations.

## 5. Partner-lender failure — the wind-down protocol

If the lender loses its license or fails (`03_...` §4.3), the agreement auto-triggers:

1. Stop new originations. 2. Stop any token issuance. 3. Move collections to an approved account. 4. Appoint backup servicer. 5. Deliver complete borrower/loan files. 6. Transfer/assign loans where legally permitted. 7. Preserve borrower payment instructions. 8. Notify investors/regulators.

**capitalYA never "takes over" the lender's regulated functions.** A partner failure is a wind-down event, not an automatic promotion into licensed lending.

## 6. Fee flows and who pays whom (Horizon 1)

| Fee | Payer | Payee | Risk posture (`03_...` §4.4/§5.2) |
|---|---|---|---|
| CRP assessment fee (flat $2.5–5K) | Borrower (or lender, per deal) | CRP | Green/Yellow — flat, non-contingent |
| Platform / workflow subscription | Lender (and/or borrower) | capitalYA CR S.A. | Green/Yellow — fixed, disclosed |
| Origination-support fee | Lender | capitalYA CR S.A. | Yellow — must not resemble creditor economics |
| Documented success fee | Lender | capitalYA CR S.A. | Yellow — disclosed, separated from interest |
| Servicing-technology fee | Lender | capitalYA CR S.A. | Green/Yellow — for delegated servicing tech |
| Interest / principal | Borrower | **Lender only** | N/A to capitalYA — never touches our accounts |

**Prohibited in Horizon 1** (creditor/fund-like economics): % of loan interest, first-loss, borrower-vs-investor spread, guaranteed minimum yield, discretionary principal participation (`03_...` §4.4).

**Tax note (`03_...` §11):** "not a lender" is a regulatory posture, not a tax exemption. capitalYA/CRP fee income earned through a CR business is taxable; obtain a transaction-level tax opinion before setting prices. Routing fees to a Cayman foundation without documented substance creates problems, not savings.

## 7. Horizon-2 expansion structure (Option C, gated)

At scale, if capitalYA ever funds, holds, pools, or syndicates, it risks becoming a lender/intermediary/issuer/fund. The durable structure (`03_...` §12) keeps functions separated across qualified parties:

1. Technology company — capitalYA. 2. Assessment company — CRP. 3. Licensed originators (per country). 4. Issuer/SPV or trust (owns receivables). 5. Regulated placement platform (distributes investments). 6. Custodian/trustee. 7. Servicer + backup servicer. 8. Fund manager/adviser (only if discretionary allocation exists). 9. Transfer agent/registry. 10. Regulated secondary venue (only where legally available).

This is **not** built in Horizon 1. It is the target architecture for tokenization/syndication and only after the relevant classifications and opinions exist.

## 8. Cross-market entity notes (secondary markets)

Per `03_...` §3 and §13, expansion uses **local licensed partners**, not self-operated venues:

- **El Salvador** — private CNAD digital-asset issuance via a **registered structurer**, ≤50 qualified investors, registered PSAD for placement/custody/trading. (Preferred tokenization path.)
- **Colombia** — partner with an existing **SFC-authorized collaborative-financing** platform; do not operate our own retail investment platform initially.
- **Brazil** — only through **CVM-registered** crowdfunding/securitization and, where applicable, **BCB-authorized** virtual-asset partners. 2027+ institutional project.

## 9. Open structural decisions (route to counsel)

These block finalizing the structure and map to `03_...` §17:

| # | Decision | Counsel question ref |
|---|---|---|
| S-1 | CRP as separate entity vs. ring-fenced service line | Q4 |
| S-2 | Does capitalYA's fee/workflow model constitute intermediation/brokerage? | Q1 |
| S-3 | Which entity is the credit provider in every customer interaction? | Q3 |
| S-4 | Which entity registers under AML Art. 15 / 15 bis; which files SARs? | Q11, Q12 |
| S-5 | Which PRODHAB registrations apply; cross-border consent language | Q13, Q14 |
| S-6 | US parent vs. CR-only holding for the cap table | (structure/tax opinion) |
| S-7 | When/whether the Cayman foundation is introduced | Q5–Q7 (Horizon 2) |

Until S-1…S-7 are answered in writing, this document is a **working structure**, not a settled one. Track resolutions in `../capital-readiness-platform/06_OPEN_QUESTIONS.md` and the counsel checklist `../capital-readiness-platform/64_...`.
