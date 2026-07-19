# 02 — Integrated Architecture: CRP × capitalYA × Partner Lender

| Field | Value |
|---|---|
| Purpose | Define exactly how the Capital Readiness Platform, the capitalYA platform, and the licensed partner lender connect — data flows, decision rights, handoff contracts, and responsibility split |
| Audience | Backend/platform engineers, integration engineers, product, compliance, partner-lender technical counterparts |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product + Platform lead |
| Dependencies | `01_CAPITALYA_PROGRAM_BRIEF.md`, `03_REGULATORY_STRATEGY_COSTA_RICA.md`, `04_ENTITY_AND_PARTNER_STRUCTURE.md`; CRP `40_SYSTEM_ARCHITECTURE.md`, `42_API_AND_INTEGRATION_SPECIFICATION.md`, `33_DATA_MODEL_AND_ENTITY_RELATIONSHIPS.md`, `27_READINESS_REPORT_SPECIFICATION.md`, `36_AUDIT_LOG_AND_EVENT_MODEL.md` |
| Governing constraint | Boundaries in CRP `02_PRODUCT_PRINCIPLES_AND_BOUNDARIES.md` and prohibitions in `03_...` §16 are binding on this architecture |
| Last updated | 2026-07-19 |

---

## 1. Purpose and scope

This document is the contract between three parties that must interoperate without any one of them stepping across a regulatory boundary:

- **CRP** produces a readiness assessment. It never approves credit.
- **capitalYA** routes, presents, and coordinates. It never lends or holds funds.
- **The partner lender** underwrites, decides, contracts, disburses, collects, and reports. It is the only creditor.

The architecture below is designed so that each boundary is enforced by *system design*, not just policy — because "the software couldn't do X" is a far stronger regulatory position than "we promised not to do X."

This document covers **Horizon 1 (the partner-lending MVP)**. Horizon-2 tokenization architecture (SPV/trust, security-token issuance, investor rails) is deferred and only sketched in §9.

## 2. Logical component map

```
┌────────────────────────────────────────────────────────────────────────┐
│ BORROWER SURFACE (SME)                                                   │
│  • intake • document upload • consent capture • remediation • status     │
└───────────────┬──────────────────────────────────────────────┬─────────┘
                │                                                │
                ▼                                                │
┌──────────────────────────────────────────────┐                │
│ CRP — ASSESSMENT PLANE (assessment entity)    │                │
│  ┌────────────────────────────────────────┐   │                │
│  │ Deterministic core (owns all math)     │   │                │
│  │  scoring engine (20/21) • gates/caps    │   │                │
│  │  (23) • evidence confidence (22)        │   │                │
│  ├────────────────────────────────────────┤   │                │
│  │ AI assist (proposes, never decides, 31) │   │                │
│  │  extraction (32) • doc intelligence (30)│   │                │
│  ├────────────────────────────────────────┤   │                │
│  │ Human review + approval (D-07)          │   │                │
│  ├────────────────────────────────────────┤   │                │
│  │ Audit log (36) — every material action  │   │                │
│  └────────────────────────────────────────┘   │                │
│  OUTPUT: Readiness Assessment Package (§4)     │                │
└───────────────┬────────────────────────────────┘                │
                │ consent-gated publish (CRP SOP 55)               │
                ▼                                                  ▼
┌────────────────────────────────────────────────────────────────────────┐
│ capitalYA — COORDINATION PLANE (technology company)                     │
│  • opportunity intake from CRP  • partner routing/matching (non-binding) │
│  • workflow + task orchestration • status + notifications                │
│  • servicing-technology (delegated) • investor surface (Horizon 2 only)  │
│  NO custody • NO credit decision • NO guarantee                          │
└───────────────┬────────────────────────────────────────────────────────┘
                │ opportunity handoff (§5)  +  decision webhook (§6)
                ▼
┌────────────────────────────────────────────────────────────────────────┐
│ PARTNER LENDER — CREDIT & FUNDS PLANE (licensed / supervised)           │
│  • independent underwriting • approve/decline • set terms • sign contract│
│  • KYC/AML of record • disbursement • collections • regulatory reporting │
│  • funds move only through lender / regulated PSP accounts               │
└────────────────────────────────────────────────────────────────────────┘
```

**Plane rule:** data crosses planes only through the defined interfaces in §4–§6. Funds never enter the Assessment or Coordination planes. Credit decisions never originate in the Assessment or Coordination planes.

## 3. Decision-rights matrix (who is allowed to do what)

| Action | CRP | capitalYA | Partner lender | Enforced by |
|---|:--:|:--:|:--:|---|
| Collect borrower documents | ✅ | ✅ (transport) | — | CRP intake SOP 51 |
| Extract/verify evidence | ✅ | — | — | CRP 30/32 |
| Compute readiness score/flags | ✅ | — | — | CRP 20/21/23 (deterministic) |
| Approve the **assessment** | ✅ (human) | — | — | CRP D-07, SOP 53 |
| Publish to a lender | ✅ (on consent) | ✅ (transport) | — | CRP SOP 55; consent ledger |
| Rank/match opportunities to lenders | — | ✅ (non-binding, explainable) | — | Boundaries §2 |
| **Underwrite / approve credit** | ❌ | ❌ | ✅ | Partner agreement; system has no approval path |
| **Set final loan terms** | ❌ | ❌ | ✅ | Partner agreement |
| **Sign loan contract** | ❌ | ❌ | ✅ | Partner agreement |
| **Disburse / receive funds** | ❌ | ❌ | ✅ | No capitalYA money rails exist |
| KYC/AML of record + SAR filing | ❌ (complementary screening only) | ❌ (complementary screening only) | ✅ | `03_...` §8; responsibility matrix §7 |
| Collections / enforcement | ❌ | (servicing tech only) | ✅ | Partner agreement |
| Regulatory reporting on loans | ❌ | ❌ | ✅ | Partner agreement |

`✅` allowed / owns · `❌` prohibited by design · `—` not applicable

## 4. The Readiness Assessment Package (CRP → capitalYA contract)

CRP's output is a versioned, signed, immutable **Readiness Assessment Package (RAP)**. It is the only thing that crosses from the Assessment plane into the Coordination plane, and only after recorded borrower consent (CRP SOP 55, consent ledger `ENT-24`).

### 4.1 Contents

| Element | Source doc | Notes |
|---|---|---|
| `assessment_id`, `borrower_id` | CRP 33 | Stable identifiers |
| `readiness_score` (0–100) + band | CRP 20, 21 | Deterministic; **not** a credit score/PD |
| `dimension_scores` (8 dimensions) | CRP 21 | A1…H5 control rollups |
| `evidence_confidence` | CRP 22 | Separate axis — **never blended into score** |
| `flags` (severity S1–S4) | CRP 23 | Integrity/eligibility signals |
| `gates_and_caps_applied` | CRP 23 | Why a score was limited |
| `external_risk_context` | CRP 20 | Reported, never a hidden penalty |
| `evidence_manifest` | CRP 26, 35 | Document types, tiers, freshness, hashes |
| `remediation_items` | CRP 28 | What would raise readiness |
| `human_approval` | CRP D-07 | Approver, timestamp, reason |
| `methodology_version` | CRP 20 §10 | Scoring-model version for reproducibility |
| `limitation_statement` | CRP 02 §3 | Mandatory, verbatim, EN + counsel-validated ES |
| `assessment_date`, `expiry` | CRP 27 | Refresh policy |

### 4.2 The limitation statement travels with every RAP

Non-removable, per CRP `02` §3:

> The Capital Readiness Score measures how prepared this business is for capital review. It is not a credit score, a probability of default, a loan approval, an investment recommendation, or a guarantee of funding. Capital providers must perform their own independent underwriting, diligence, and approval process.

This is what keeps CRP outside credit-rating-agency regulation (`03_...` §5.1). Any interface that strips it is non-compliant.

### 4.3 What the RAP deliberately does NOT contain

- No approval, decline, or "recommended amount."
- No interest rate, term, or price.
- No probability of default or expected-loss number.
- No instruction to the lender. The lender consumes the RAP as **decision-support input to its own independent underwriting**, nothing more.

## 5. Opportunity handoff (capitalYA → partner lender)

capitalYA presents a consented RAP to one or more partner lenders as an **opportunity**. This is transport + presentation + non-binding ranking — not a recommendation to approve.

- **Matching** may rank lenders/opportunities using explainable, non-binding criteria (product fit, ticket size, sector appetite). It must never imply approval or use prohibited "approved/pre-approved/investment-grade" language (CRP `02` §3).
- **Consent scope** is enforced: a lender sees only what the borrower consented to share, for the purpose consented to (`03_...` §9.1; CRP consent ledger).
- **No exclusivity leakage:** presenting to a lender does not transfer any decision authority to capitalYA.

## 6. Decision + servicing callbacks (partner lender → capitalYA)

The lender's independent decision returns to capitalYA purely as **status data** for workflow and borrower notification — capitalYA records it, it does not make it.

| Callback | Payload (status only) | capitalYA may do | capitalYA may NOT do |
|---|---|---|---|
| `decision.recorded` | approved / declined / more-info, by lender, timestamp | Notify borrower; update workflow; log audit event | Generate, alter, or infer the decision |
| `terms.recorded` | rate, amount, schedule (as set by lender) | Display to borrower; store for servicing tech | Set, negotiate, or bind terms |
| `disbursement.recorded` | lender-confirmed funding event | Update status; start servicing-tech clock | Touch or route funds |
| `servicing.event` | payment received/missed (from lender/PSP) | Reflect status; trigger reminders (delegated) | Collect, enforce, or hold funds |

All callbacks are **records of the lender's actions**, mirroring CRP's audit-event discipline (CRP 36). Payment status must never flow back into CRP scoring (CRP `02` P10 / boundary §2.9).

## 7. AML/CFT responsibility split (the part everyone gets wrong)

Per `03_...` §8.1: the partner lender is the **primary regulated AML reporting entity**, but capitalYA/CRP cannot operate blind because they collect identity data and see borrowers first.

| Function | CRP / capitalYA | Partner lender |
|---|---|---|
| Identity capture & UBO collection | ✅ collect | ✅ verify of record |
| PEP / sanctions / adverse-media screening | ✅ complementary | ✅ of record |
| Source-of-funds / wealth review | flag/escalate | ✅ of record |
| Transaction monitoring | pre-handoff signals | ✅ of record |
| Escalation of suspicious conduct | ✅ to lender MLRO (no tipping-off) | ✅ investigate |
| SAR / suspicious-transaction filing | only if own registration requires | ✅ files |
| Sanctions lists screened | UN, CR/local, OFAC (where US nexus), partner-required | same, of record |
| Record retention | ✅ (own records) | ✅ of record |

The partner agreement must nail down who files, who investigates, who retains the file, and how capitalYA transmits observations without tipping off (`03_...` §8.1). "The lender owns all AML" is not an acceptable design.

## 8. Data protection & residency (design constraints)

From `03_...` §9.1 and CRP `44_SECURITY_PRIVACY_AND_DATA_PROTECTION.md`:

- **Lawful basis:** express, precise, Spanish-language consent (Ley 8968), including **international-transfer consent** for US-cloud processing.
- **US cloud is permitted** with: recipient/country disclosure, controller–processor agreement, subprocessor list, encryption in transit + at rest, incident response, retention/deletion schedule, DSAR procedures, PRODHAB registration analysis. (CRP decision D-23: US cloud + consent.)
- **Hard rule:** **no personal or financial data on any public blockchain.** Horizon-2 tokenization stores PII off-chain; only non-personal commitments/hashes may ever be on-chain, and only after counsel sign-off.
- **ZK proofs** (Horizon 2) reduce disclosure but do **not** replace consent, accuracy, correction rights, or lawful collection of the source data (`03_...` §9.1).

## 9. Where the PRD's on-chain architecture maps (Horizon 2, deferred)

The PRD/specs describe a Diamond-pattern protocol (ERC-3643 invoice tokens, ERC-4626 vaults, on-chain RiskEngine, DAO, DEX). None of it ships in Horizon 1. When it is revisited, it maps onto this architecture as follows — and only behind the regulatory gates in `03_...`:

| PRD/spec component | Horizon-1 status | Horizon-2 reframe (gated) |
|---|---|---|
| `RiskEngineFacet` (on-chain scoring) | **Off-chain CRP** does all scoring; nothing on-chain | On-chain may store only a signed RAP hash/attestation, never PII or the model |
| `InvoiceTokenFacet` (ERC-3643) | Not issued | Only after SUGEVAL classification (CR) or via CNAD structurer (SV); private ≤50 qualified investors |
| `LoanVaultFacet` (ERC-4626) | Not deployed; no pooled capital | Only via regulated SPV/trust + placement partner; not capitalYA-custodied |
| `GovernanceFacet` / DAO | Not used for anything customer-facing | Governance of protocol IP only; no soliciting local investors (`03_...` §2) |
| DEX / secondary trading | **Prohibited** | Only through a licensed venue where legally available (`03_...` §6) |
| Multi-oracle (ATV/bank APIs) | Used as **verification inputs to CRP**, off-chain | May feed on-chain attestations later |
| ZK identity | Off-chain verification only | Privacy-preserving proofs, PII stays off-chain |

**Interpretation rule:** ERC-3643 is a transfer-control tool, not a license, prospectus, assignment, or bankruptcy opinion (`03_...` §4.1, §6.1). Smart-contract "compliance by default" is one control layer among many — never the legal basis.

## 10. Failure and continuity design

Built in from day one (`03_...` §4.3):

- **Lender loses its license →** stop new originations and any token issuance; move collections to an approved account; appoint backup servicer; deliver complete borrower/loan files; preserve payment instructions; notify investors/regulators. capitalYA **does not** take over regulated lending — a partner failure is a wind-down event, not a promotion.
- **CRP model error →** version-controlled models, evidence log per score, human approval, borrower correction/appeal, reassessment after remediation (CRP 57; `03_...` §5.3).
- **Servicing interruption →** backup servicer and records-export obligations in the partner agreement (`04_...`).

## 11. Interface build order (maps to the launch plan)

| Interface | Owner | Depends on | Target window |
|---|---|---|---|
| Borrower intake + consent capture | CRP/capitalYA | CRP SOPs 51/55, consent ledger | Aug 2026 |
| RAP generation + signing/export | CRP | CRP 27, 33, 36 | Aug 2026 |
| Opportunity handoff to lender | capitalYA | RAP, consent scope | Aug 2026 |
| Lender decision/terms callbacks | capitalYA ↔ lender | Partner agreement, lender systems | Aug 2026 |
| AML escalation channel | capitalYA ↔ lender MLRO | Responsibility matrix (§7) | Aug 2026 |
| Servicing-tech status feed | capitalYA ↔ lender/PSP | Partner agreement | Sep 2026 (pilot) |
| Horizon-2 token/SPV rails | deferred | SUGEVAL/CNAD classification | Q4 2026+ |

Everything in the Aug/Sep rows must exist for the **closed pilot** (`01_...` §10). Horizon-2 rails are explicitly out of MVP scope.
