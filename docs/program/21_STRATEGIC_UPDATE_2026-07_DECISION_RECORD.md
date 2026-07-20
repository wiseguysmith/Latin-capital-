# 21 — Strategic Update & Decision Record (July 2026, Partner Memo)

| Field | Value |
|---|---|
| Purpose | Record the partner strategic update (hub-and-spoke framework, phased roadmap, three locked Costa Rica guardrails), state what is **adopted** into the program docs, and what is **flagged** for counsel or future research |
| Audience | Founders, counsel, engineering, compliance |
| Status | Adopted decisions applied across the suite (see §5); flagged items unresolved |
| Version | 1.0.0 |
| Owner | Founder |
| Source | Partner strategic memo received 2026-07-19 ("Strategic Project Update & Roadmap: capitalYA & CRP") |
| Precedence | For Phase 1/2 fee, notice, and privacy mechanics this record **governs** and the suite has been updated to match. For Phase 3, nothing here overrides `03_...` — see §4. |
| Last updated | 2026-07-19 |

---

## 1. Phase mapping (partner memo ↔ program docs)

| Partner memo | Program equivalent | Status |
|---|---|---|
| **Phase 1 — CRP as standalone B2B SaaS** risk/data engine | The CRP suite already stands alone by design | ✅ Consistent — no change needed |
| **Phase 2 — capitalYA marketplace**, lender carries 100% credit risk | Horizon 1 (`01` §2): partner-originated, non-custodial | ✅ Consistent — identical boundary |
| **Phase 3 — offshore fund pivot** (Dubai/VARA/ADGM or Cayman SPV, stablecoin drawdowns, smart-contract waterfall) | A **variant** of Horizon 2 / Option C (`03` §12) — but materially different from the researched El Salvador/Colombia/CR tokenization paths (`03` §15) | ⚠ **Flagged** — see §4 |

## 2. Locked Costa Rica guardrails — ADOPTED

### D-P1 · Fee & usury alignment: strict B2B inter-corporate invoicing ("Option A")

- capitalYA and CRP **never charge mandatory transactional or technology fees to the end SME borrower**. The borrower faces only the lender's disclosed rate and the lender's own charges.
- All platform economics (CRP assessment fee, platform/workflow fees, success fees, servicing-technology fees) are **billed B2B to the lender**.
- Rationale: under Law 9859, mandatory borrower-side fees for access to credit count toward the BCCR usury caps (partner-reported ≈30.11% USD / ≈36.48% CRC for standard corporate loans — **verify per semester**). B2B billing removes our fees from the borrower's cap arithmetic and reinforces the non-creditor stance.
- **Counsel confirmation still required (sharpened Q):** Law 9859's anti-evasion principle — confirm that lender-paid platform fees, absorbed into lender pricing, are not recharacterized into the borrower's effective-rate test; and that no disclosure duty arises for pass-through economics.
- Docs updated: `05` (§3 item 16), `07` §4.6, `08` §§2–5, `10` §5, `11` §2, `12` §3, `17`, `18`, both mockups.

### D-P2 · Factoring perfection: automated assignment notice (Ley 9244)

- The moment the lender **accepts and finances** an invoice, the platform **programmatically generates and dispatches a digitally signed notice of assignment (notificación de cesión) to the underlying corporate debtor**, in the **lender's name** (the lender is the assignee; capitalYA is dispatch infrastructure).
- Rationale: under the Ley de Garantías Mobiliarias (Law 9244), formal debtor notification perfects the assignment and prevents payments legally reverting to the SME.
- **Counsel confirmation required:** exact form, content, signature, and delivery-evidence requirements for a valid notice; whether registry filing is also required per receivable type (`03` §17 Q8).
- Docs updated: `06` §8.2, `07` §4.5 (event), `19` (SCR-D8 note), `20` Phase 1.

### D-P3 · Data privacy: rigid consent + upfront PRODHAB registration

- Onboarding uses a **mandatory, non-pre-checked, Spanish-language informed-consent form** explicitly disclosing transmission, processing, and storage on US cloud infrastructure. (Consent design in `13` and the borrower mockup already matched; now locked.)
- **PRODHAB database registration filings are executed upfront**, concurrent with Phase 1 entity incorporation — upgraded from "registration analysis required."
- Docs updated: `13` §§2, 5; `09` §3/§6; counsel brief.

## 3. Standing instructions — ADOPTED

1. capitalYA and CRP are positioned strictly as **technology infrastructure and assessment providers** through Phases 1–2 (consistent with `05`; "unregulated" is our posture *to defend via counsel*, not a fact to assert to regulators).
2. All fee/revenue mechanics are **B2B inter-corporate**; no borrower-facing financial surcharges anywhere in the suite, contracts, or UI.
3. capitalYA and CRP remain **structurally ring-fenced** (clean cap tables, separable IP, arm's-length inter-company agreements) to allow a future roll-up into a Phase 3 parent **without contaminating** the Phase 1/2 regulatory posture. (Reinforces `04` §3 S-1 and the Horizon rule `01` §2.)

## 4. Phase 3 — FLAGGED, not adopted into operating docs

The offshore fund model (Dubai/VARA/ADGM or Cayman SPV pooling global capital; local lenders as funded origination/servicing arms; programmatic stablecoin drawdowns and repayment waterfalls) is recorded as **strategic intent only**. It is **not** incorporated into any Phase 1/2 legal, technical, or operational document, because:

1. It is **not "unregulated"**: a capital-pooling SPV/fund + stablecoin facilities + cross-border wholesale lending implicates fund regulation, VASP licensing (VARA/ADGM have explicit regimes), AML at the fund layer, and the lending-perimeter rules of every domestic market it funds. The cited analogues (Goldfinch, Centrifuge) both operate under — and have tested the limits of — securities regimes.
2. It **diverges from researched paths**: `03` §15 selected El Salvador (CNAD) / Colombia (SFC) / Costa Rica-private as the tokenization options. Dubai/Cayman-fund-to-lender wholesale funding is a different structure with different questions — **none researched yet**. Mexico as a target market is also unresearched.
3. Converting partner lenders into **funded origination arms** changes the Phase 2 economics ("lender carries 100% risk on its own balance sheet") into forward-flow/wholesale-facility relationships — which our own `03` §12 warns transforms capitalYA's regulatory profile.

**Required before Phase 3 enters any operating document:** a dedicated regulatory research cycle (VARA/ADGM vs. Cayman; wholesale-facility structures into CR/CO/MX; stablecoin drawdown legality per market; fund marketing rules), counsel opinions, and an explicit Founder decision superseding `03` §15's pathway choice. Until then, the `01` Horizon rule applies: **no Phase 3 language in borrower-, lender-, or regulator-facing materials.**

## 5. Suite changes applied under this record

| Doc | Change |
|---|---|
| `05` | Prohibition #16 appended: no mandatory borrower-facing fees (Phase 1/2) |
| `06` | §8.2 added: automated Ley 9244 assignment-notice dispatch on financing |
| `07` | Fee-config `payer` constrained to `lender` for mandatory fees; `assignment.notice_dispatched` event added |
| `08` | Payer rules and cap treatment rewritten for B2B-only billing; Law 9859 note |
| `10` §5, `12` §3, `17`, `18` | All fee payers → lender (B2B); borrower-payer options removed |
| `11` §2 | Borrower cost disclosure simplified: lender's charges are the entire borrower-facing cost |
| `13` | Non-pre-checked consent locked; PRODHAB moved to upfront execution |
| `09` | Package reflects executed PRODHAB filings + B2B fee model |
| `20` | Phase 1 slice adds assignment-notice automation; PRODHAB filing task |
| `briefs/COUNSEL_ENGAGEMENT_BRIEF` | Two sharpened questions added (9859 anti-evasion; 9244 notice form) |
| Mockups | Borrower: fee card replaced with no-borrower-fees statement. Lender: cap composition reflects B2B billing |

## 6. What this memo did NOT resolve (unchanged open items)

- **No lender identified** — the 17 diligence questions (`briefs/LENDER_DILIGENCE_QUESTIONS.md`) remain fully open; the three lender meetings still gate the pilot.
- **No counsel opinions** — all 18 questions in `03` §17 stand, two now sharpened (see §2).
- **Phase 3 structure** — flagged per §4; requires its own research cycle.
