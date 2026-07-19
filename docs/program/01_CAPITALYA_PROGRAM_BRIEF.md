# 01 — capitalYA Program Brief

| Field | Value |
|---|---|
| Purpose | Single source of truth for what the capitalYA program is, how CRP fits inside it, and what we are actually building first |
| Audience | Founders, product, engineering, legal/compliance, capital partners, prospective investors |
| Status | Draft v1.0 — governing narrative for the unified program |
| Version | 1.0.0 |
| Owner | Founder |
| Dependencies | `02_INTEGRATED_ARCHITECTURE.md`, `03_REGULATORY_STRATEGY_COSTA_RICA.md`, `04_ENTITY_AND_PARTNER_STRUCTURE.md`, and the entire Capital Readiness Platform suite (`../capital-readiness-platform/`) |
| Source references | capitalYA PRD v1.0 (`sources/capitalya_prd_v1.md`); capitalYA Technical Specifications v1.0 (`sources/capitalya_technical_specifications_v1.md`); capitalYA + CRP Regulatory Strategy (`03_...`) |
| Governing constraint | Where the PRD/specs and the Regulatory Strategy conflict, **the Regulatory Strategy governs the near-term plan.** |
| Last updated | 2026-07-19 |

---

## 1. One-paragraph summary

**capitalYA** is a financial-technology program that makes capital move faster, cheaper, and more transparently for Latin American SMEs — starting in Costa Rica. It has two software products and one deliberate legal posture:

1. **The Capital Readiness Platform (CRP)** — an assessment engine that organizes, verifies, scores, and explains how prepared a business is to receive capital. (Already fully specified: 70+ documents in `../capital-readiness-platform/`.)
2. **The capitalYA platform** — the origination-support, workflow, matching, and (later) tokenization layer that connects a readiness-assessed borrower to capital.
3. **A non-lender, non-custodian posture** — capitalYA coordinates; a **licensed partner lender** makes every credit decision, signs every loan, and moves every dollar. capitalYA does not take deposits, pool investor money, guarantee repayment, or become the creditor.

The long-term ambition (from the PRD) is a hybrid RWA + consumer-debt protocol with tokenized invoices and global yield. The near-term reality (from the Regulatory Strategy) is a **compliant partner-lending and assessment engine** that earns the right to tokenize later. This document holds both truths and keeps them in the right order.

## 2. The two-horizon framing

The program has repeatedly gotten into trouble by treating the north-star vision as the launch plan. It is not. We separate the two explicitly and never blur them again.

| | **Horizon 1 — What we build first (MVP)** | **Horizon 2 — North star (why it's worth it)** |
|---|---|---|
| Product | Readiness assessment + partner-originated SME lending | Tokenized invoice factoring, consumer debt, yield vaults |
| capitalYA's role | Technology + workflow + non-binding matching | Protocol + issuance + servicing infrastructure |
| Who lends | A licensed Costa Rican partner lender | Regulated originators + syndicated investor capital |
| Who decides credit | The partner lender (human underwriter) | Lender + risk engine, still human-gated |
| Money custody | None — lender's regulated accounts only | Regulated custodians / trustees / SPVs |
| Tokens | **None sold to the public** | Compliant security tokens via CNAD/SUGEVAL/CVM partners |
| Regulatory footprint | Operating co. + partner + privacy/AML registration | Multi-entity, multi-jurisdiction, per-market licensed |
| Timeline | 6–12 weeks to closed pilot | 18–48 months, market by market |
| Source | `03_REGULATORY_STRATEGY_COSTA_RICA.md` (governing) | `sources/capitalya_prd_v1.md` (ambition) |

**Rule:** Every roadmap, pitch, contract, and screen must make clear which horizon it describes. Horizon-2 language ("tokenized," "yield," "DeFi," "protocol," "DAO") must never appear in Horizon-1 borrower- or investor-facing materials.

## 3. The problem we're solving (unchanged from the PRD)

| Pain point | Impact |
|---|---|
| Banking oligopoly | ~4 banks control ~80% of Costa Rica's credit market — little competition, little innovation. |
| Usurious rates | SMEs pay 18–25% for working capital; consumers pay 30–45% on cards. |
| Broken timelines | SME loans take 60–90 days to approve; invoices are paid 60–180 days late. |
| Exclusion | Banks won't profitably lend below ~$50K tickets; SMEs and middle-class families are locked out. |
| Trapped capital | Local savings earn 2–4% while global investors seek 8–12% in emerging markets. |

capitalYA's thesis: most of the friction is **not** a shortage of capital — it is **poor readiness, slow verification, and opaque risk.** That is precisely what CRP fixes, and it is why CRP is the wedge, not an add-on.

## 4. Why CRP is the strategic core, not a feature

The original PRD treated credit scoring as an on-chain "RiskEngineFacet." The Regulatory Strategy and the CRP suite show why that framing is both legally risky and commercially backwards. Reframed:

- **CRP is the underwriting-readiness layer.** It standardizes borrower documentation, evidences claims, scores preparedness (0–100), flags risks, and produces a reviewable report — *before* any lender spends time underwriting.
- **CRP is what makes partner lending work.** A licensed lender will only partner with capitalYA if the borrowers arriving are pre-organized, pre-verified, and cheap to underwrite. CRP is that promise, operationalized.
- **CRP is what makes tokenization possible later.** Horizon 2 needs loan quality, documentation consistency, servicing history, and data governance. CRP generates exactly that audit trail from day one.
- **CRP stays inside its boundary.** It is an *informational readiness assessment* — not a credit rating, not an approval, not a guarantee. That boundary (already binding in `../capital-readiness-platform/02_PRODUCT_PRINCIPLES_AND_BOUNDARIES.md`) is what keeps CRP out of credit-rating-agency and securities-rating regulation. See `03_...` §5.

**In one line:** capitalYA is the marketplace and rails; CRP is the trust layer that makes the marketplace worth entering; the partner lender is the balance sheet.

## 5. The three-party operating model (Horizon 1)

```
        ┌─────────────────────────────────────────────────────────┐
        │                    BORROWER (SME)                        │
        │        submits documents, consents to sharing           │
        └───────────────────────────┬─────────────────────────────┘
                                     │
                                     ▼
        ┌─────────────────────────────────────────────────────────┐
        │   CRP — Capital Readiness Platform (assessment entity)   │
        │   organizes • verifies • scores • flags • explains       │
        │   OUTPUT: readiness report + score + evidence + flags    │
        │   NOT: approval, rating, guarantee, underwriting         │
        └───────────────────────────┬─────────────────────────────┘
                                     │  consent-gated handoff
                                     ▼
        ┌─────────────────────────────────────────────────────────┐
        │   capitalYA platform (technology + workflow + matching)  │
        │   routes • presents • tracks • coordinates servicing tech│
        │   NOT: lender, custodian, guarantor, decision-maker      │
        └───────────────────────────┬─────────────────────────────┘
                                     │  presents opportunity
                                     ▼
        ┌─────────────────────────────────────────────────────────┐
        │   PARTNER LENDER (licensed / supervised)                 │
        │   underwrites • approves/declines • sets terms • signs   │
        │   contract • disburses • collects • reports • owns AML   │
        │   >>> THE ONLY PARTY THAT LENDS AND HOLDS FUNDS <<<      │
        └─────────────────────────────────────────────────────────┘
```

Full data flows, decision rights, and the responsibility matrix are in `02_INTEGRATED_ARCHITECTURE.md`. Entity and contract structure is in `04_ENTITY_AND_PARTNER_STRUCTURE.md`.

## 6. What we are NOT building in Horizon 1

Directly from `03_...` §1 and §16 — these are non-negotiable "do not cross" lines for the MVP:

- No public retail sale of tokenized loans or invoices.
- No DEX or secondary trading venue.
- No capitalYA-controlled wallets or pooled investor accounts.
- No automated credit approval without lender review.
- No Cayman DAO marketing investments to Costa Rican residents.
- No tokenized loan participations without written SUGEVAL classification.
- No capitalYA- or CRP-branded "credit rating" used to sell investments.
- No personal or financial data written to a public blockchain.

If a design, deck, or contract implies any of the above for the MVP, it is wrong and must be corrected against `03_...`.

## 7. Product scope by phase (reconciled)

The PRD's product menu is retained as the **ambition**; the phase gating is rewritten to match the Regulatory Strategy.

| Phase | PRD label | Reconciled scope | Structure |
|---|---|---|---|
| **P1 — Readiness + Partner MVP** | "Bootstrap / Validate" | SME invoice factoring & working-capital loans, originated by the partner lender, gated by CRP readiness. Closed pilot, limited borrower/loan count, lender's accounts only. | Partner-originated, non-custodial (`03_...` §2) |
| **P2 — Private tokenization pilot** | "Scale Corp" | First tokenized invoice/loan participations — **private, institutional, ≤50 qualified investors**, via a registered structurer. Not in Costa Rica retail. | El Salvador CNAD path preferred (`03_...` §6.2, §15 Path A) |
| **P3 — Regulated retail/investment** | "Consumer Launch" | Broader investor access through an **existing licensed platform** (e.g., Colombia SFC collaborative-financing partner), not a self-operated venue. | Partner platform (`03_...` §6.3) |
| **P4 — Institutional scale** | "Mortgage / Regional" | Securitization and institutional syndication via SPV/trust + regulated placement, market by market. | Option C multi-entity (`03_...` §12) |

Consumer lending (personal loans, cards, mortgages) from the PRD's "Phase 2" remains a **later horizon** and inherits Costa Rica's consumer-credit and interest-cap regime (`03_...` §10). It is not part of the near-term SME MVP.

## 8. How this changes the PRD's key assumptions

The PRD made several assumptions the Regulatory Strategy corrects. These are load-bearing — carry them into every downstream doc.

| PRD assumption | Corrected position | Source |
|---|---|---|
| capitalYA CR S.A. is a "SUGEVAL-regulated securities issuer" holding a lending license | capitalYA is a **technology/assessment company**; the **partner lender** holds the license and is the creditor | `03_...` §2, §3.1 |
| Sandbox gives a "regulatory testing environment" / exemption | Costa Rica has a **consultation center (CIF), not a waiver-based sandbox**; do not assume exemption | `03_...` §7.1 |
| Tokenized invoices → register as "small issuer," 499 investors | Public token = securities offering needing SUGEVAL authorization; use **private ≤50 qualified investors** or another market | `03_...` §6.1 |
| "Ley 8959 Sistema de Banca para Todos" is the enabling law | Correct statute is **Law 8634, Sistema de Banca para el Desarrollo**; it is not a DeFi exemption | `03_...` §3.1 |
| SAR/AML threshold "$10,000 CRC" | ₡10,000 is trivial; thresholds must be mapped to entity/transaction/currency by counsel | `03_...` §8.1 |
| On-chain credit scoring, ZK proofs on public chain | **No PII on public chains**; ZK reduces disclosure but does not legalize source data or replace consent | `03_...` §9.1 |
| Cayman Foundation gives "regulatory arbitrage" / "sovereign immunity" | A foreign entity is still captured when it offers into Costa Rica; **decentralization is not an invisibility cloak** | `03_...` §2 |
| "Not a lender" avoids tax | Regulatory position ≠ tax exemption; **fee income is taxable** | `03_...` §11.1 |

## 9. Commercial model (Horizon 1, safe-by-design)

Fee structure follows the low-risk menu in `03_...` §4.4 — fixed and disclosed, never resembling a creditor's or fund manager's economics:

- **CRP assessment fee** — flat **$2,500–$5,000** per assessment (document analysis, readiness review, remediation plan). Not contingent on financing closing. (`03_...` §5.2; CRP decision D-21 flat fee.)
- **capitalYA platform / workflow fee** — fixed technology subscription and/or lender-paid origination-support fee.
- **Documented success fee** — for completed introductions, disclosed, separated from interest income.
- **Servicing-technology fee** — where servicing functions are contractually delegated.

Explicitly avoided in Horizon 1 (creditor/fund-like economics): percentage of loan interest, first-loss return, borrower-vs-investor spread, guaranteed minimum yield, discretionary participation in principal.

## 10. Milestones (reconciled with the Regulatory Strategy's launch plan)

| Window | Objective | Key deliverables |
|---|---|---|
| **Jul 20–31, 2026** | Legal foundation | Freeze token dev; full legal activity map + data-flow diagram; select CR banking & securities counsel; shortlist 3 supervised lenders; define CRP as assessment-only; begin PRODHAB analysis; AML responsibility matrix; draft partner term sheet |
| **Aug 1–31, 2026** | Partner + compliance | Negotiate lender agreement; banking-law legal opinion; CIF consultation; SUGEVAL pre-classification discussion; borrower consent & privacy docs; CRP methodology governance; human underwriting handoff; lender payment-rail integration; bind cyber/E&O; security assessment |
| **Sep 1–30, 2026** | Closed pilot | Launch closed Costa Rica pilot; limited borrowers/loans; lender's accounts only; **no tokens, no retail investors**; manually audit first assessments; test lender-failure/servicing-continuity |
| **Oct–Dec 2026** | Tokenization pathway | Choose one: El Salvador CNAD private issuance (Path A, preferred), Colombia licensed platform (Path B), or Costa Rica private-institutional after SUGEVAL classification (Path C) |

Full detail: `03_...` §15. Cost ranges: partner MVP **$40K–$100K**; private token pilot **$75K–$200K**; full public securities offering **$250K–$750K+**.

## 11. Naming and identity

- The program and public brand is **capitalYA** (formerly "TITANO" in the PRD/specs; that name is retired).
- **CRP / Capital Readiness Platform** is the assessment product name, usable with lenders and borrowers.
- The governance/protocol token concept ("TITANO token" in the PRD) is **Horizon 2 and unnamed for now**; it must not appear in MVP materials.

## 12. How to use this document

1. **Product/engineering:** build Horizon 1 only. Treat `sources/` as vision context, not a spec. Treat CRP docs as the live spec for the assessment engine.
2. **Legal/compliance:** `03_...` is your working document; the 18 counsel questions in `03_...` §17 are the gate before pilot.
3. **Fundraising/partners:** you may show the Horizon-2 ambition, but every deck must label horizons per §2 and carry the CRP limitation statement when a readiness score is shown.
4. **Any conflict** between this brief, the PRD/specs, and the Regulatory Strategy resolves in favor of the **Regulatory Strategy (`03_...`)** for anything near-term, and this brief for scope/sequencing. Log conflicts in `../capital-readiness-platform/06_OPEN_QUESTIONS.md`.
