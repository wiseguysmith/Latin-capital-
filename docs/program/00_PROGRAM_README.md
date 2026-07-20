# 00 — capitalYA Program Documentation (README)

| Field | Value |
|---|---|
| Purpose | Index and reading order for the unified capitalYA program: the capitalYA platform, the Capital Readiness Platform (CRP), the partner-lender model, and the governing regulatory strategy |
| Audience | Everyone — founders, product, engineering, legal/compliance, capital partners, investors |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Founder |
| Last updated | 2026-07-19 |

---

## 1. What this layer is

This `docs/program/` layer sits **above** the Capital Readiness Platform suite (`../capital-readiness-platform/`). CRP is a complete, self-consistent product specification (70+ documents). This program layer explains how CRP fits inside the larger **capitalYA** program — a partner-originated lending + capital-readiness business for Latin American SMEs, starting in Costa Rica.

It exists because the CRP suite was written as a standalone assessment platform, and the capitalYA vision + regulatory strategy were developed separately. Rather than rewrite 70 internally-consistent documents, this layer **unifies** them and records the governing decisions. The CRP boundaries (assessment-only, not a lender, human-approved, no public token) are fully compatible with — and in fact required by — the regulatory strategy, so no CRP document had to be contradicted.

## 2. The one thing to understand

The program runs on **two horizons that must never be blurred**:

- **Horizon 1 (build now):** a compliant, non-custodial, **partner-originated** SME lending + readiness-assessment engine. capitalYA is technology; CRP is assessment; a **licensed partner lender** is the only party that lends, decides credit, and holds funds.
- **Horizon 2 (north star):** tokenized invoice factoring, yield vaults, and regional scale — built only after the compliant engine and its data/servicing history exist, and only behind the right licenses/partners.

The original PRD/specs describe Horizon 2. The **Regulatory Strategy (`03`) governs Horizon 1** and wins any near-term conflict.

## 3. Documents in this layer

| # | Document | Read it for |
|---|---|---|
| `00` | This README | Index, reading order |
| `01` | **capitalYA Program Brief** | The unified vision, two-horizon framing, how CRP is the strategic core, reconciled scope & milestones — **start here** |
| `02` | **Integrated Architecture** | How CRP ↔ capitalYA ↔ partner lender connect: data flows, decision rights, the Readiness Assessment Package, AML split |
| `03` | **Regulatory Strategy (Costa Rica + 3 markets)** | **Governing compliance document.** Licensing, partner model, tokenization/securities, AML, data protection, tax, timelines, do-not-cross lines, counsel questions |
| `04` | **Entity, Partner & Contract Structure** | Legal entities, partner-lender contract terms, fee flows, wind-down protocol, Horizon-2 structure |
| `05` | **MVP Regulatory Boundary Statement** | The canonical, quotable "what capitalYA/CRP is and is not" — the anchor `06`–`09` cite |
| `06` | **Partner Lender Portal & Integration Spec** | Portal-first surface (single lender), phased API, ownership-annotated state machine, settlement, disputes, SLAs |
| `07` | **CRP ↔ capitalYA Data Exchange & Validation** | Signed event-driven integration and the **canonical JSON schemas** for the whole program |
| `08` | **Fee Policy, Catalogue & Pricing Model** | Fee taxonomy, interest-cap rule, zero-rate configurable pricing; numbers deferred to pilot schedules |
| `09` | **Costa Rica Regulatory Engagement Package** | CIF-first consultation plan, specific regulator questions, 5-stage submission sequence |
| `10` | **Partner Lender Agreement — Term Sheet** | Non-binding heads of terms; responsibility matrix; deferred commercial terms; definitive-agreement clause list |
| `11` | **Borrower Disclosures** | Consumer-credit disclosure content + readiness-score rights (counsel + certified Spanish required) |
| `12` | **CRP Assessment Agreement** | Assessment-only scope, limitation statement, flat non-contingent fee, liability protections |
| `13` | **Data Processing & Consent** | Ley 8968 notice/consent, controller-processor DPA, US-cloud controls, PRODHAB analysis |
| `14` | **AML/CFT Escalation Procedure** | Complementary controls; escalation to lender MLRO; no tipping off; no auto sanctions clearance |
| `15` | **Complaint & Dispute SOP** | Routing matrix operationalized; acknowledgement/resolution flow; CRP appeal linkage |
| `16` | **Lender-Failure & Backup-Servicing Plan** | Wind-down triggers/actions; backup servicer; what capitalYA may/may not do |
| `17` | **Pilot Fee Schedule v1.0 (Template)** | Number-free until partner deal + cap test + tax opinion; instantiates `08`/`07` |
| `18` | **Partner Commercial Schedule (Template)** | Negotiated lender-side economics — contract annex to `10`; boundary-constrained; filled at signing |
| `19` | **Lender Portal Screen Specification** | SCR-D series (11 screens) extending the CRP screen registry — the lender decisioning portal mapped to the `06` state machine |
| `briefs/` | Working briefs | Consistency review (PASS), counsel engagement brief, lender pitch one-pager |
| `mockups/` | Clickable prototypes | `lender-portal-mockup.html` — interactive demo of the SCR-D core flow with the role-gated boundary made visible |
| `sources/` | Archived founder inputs | Original PRD, Technical Specifications, and research brief — **vision context, not build specs** |

**Tier-1 build set (`05`–`09`)** is the concrete MVP documentation: boundary → lender integration → data/schemas → fees → regulator engagement. Reflects the scope decisions of a **single pilot lender**, a **portal-first** surface, capitalYA holding **no funds** and making **no credit decision**.

**Tier-2 build set (`10`–`17`)** are the operational, legal, and commercial artifacts for the pilot — **working drafts/templates.** The agreements require Costa Rican counsel before execution; Spanish operative texts require certified legal drafting; the fee schedule stays number-free until the partner deal, interest-cap test, and tax opinion are done.

## 4. Reading order by role

- **Founder / leadership:** `01 → 03 → 04 → 02`
- **Product / engineering:** `01 → 02 → ../capital-readiness-platform/01 → 02 → 20 → 40`
- **Legal / compliance:** `03 → 04 → ../capital-readiness-platform/02 → 64 → 60`
- **Capital partner / investor diligence:** `01 → 03 → 04`
- **Anyone touching the assessment engine:** this `01`+`02`, then the full CRP suite

## 5. Governing order of precedence

When documents conflict:

1. For any **near-term / regulatory / structural** question → **`03` Regulatory Strategy** wins.
2. For **scope, sequencing, and horizon framing** → **`01` Program Brief** wins.
3. For **assessment-engine behavior** (scoring, gates, evidence, reports, SOPs) → the **CRP suite** wins (its own precedence rules apply: lower-numbered Foundation/Methodology docs win).
4. The **PRD and Technical Specifications in `sources/`** are the lowest precedence — vision only, superseded on every near-term point.

Log unresolved conflicts in `../capital-readiness-platform/06_OPEN_QUESTIONS.md`.

## 6. Non-negotiable invariants (program-wide)

Carried from `03` §16 and CRP `02`:

1. capitalYA/CRP never receive borrower or investor principal into their accounts.
2. Only the **licensed partner lender** approves credit, sets final terms, signs, disburses, and collects.
3. CRP produces an **informational readiness assessment**, never a credit rating, approval, or guarantee — and always carries the limitation statement.
4. **No tokens sold to the public**, no DEX, and **no personal/financial data on any public blockchain** in Horizon 1.
5. Deterministic services own all scoring math; AI assists and explains but never decides; **humans approve** every material outcome.
6. Payment/fee status never influences a readiness result.
7. Nothing marked "requires counsel validation" is settled until counsel confirms it in writing (`03` §17).

## 7. Status & what's next

The program layer (`00`–`04`) and the **Tier-1 MVP build set (`05`–`09`)** are complete: boundary statement, partner-lender portal/integration spec, CRP↔capitalYA data exchange + canonical schemas, fee policy/pricing model, and the Costa Rica regulatory engagement package.

**Tier-2 (`10`–`18`) — drafted as working templates, now complete as a set.** All require external inputs to finalize: partner negotiation (term sheet → definitive agreement via counsel), Costa Rican counsel review of all agreements/disclosures, certified Spanish drafting, PRODHAB analysis, and the interest-cap test + tax opinion before `17`/`18` take real numbers. **The documentation program has no remaining internal blockers — every open item now waits on a real-world counterpart: counsel, the partner lender, or a regulator.**

**Background only (must not delay the Costa Rica MVP):**
- El Salvador (CNAD), Colombia (SFC), Brazil (CVM/BCB) market playbooks.
- Investor messaging; competitive landscape; public token economics.

**Deferred until Horizon 2 is authorized:** Investor Waterfall Specification; tokenization/oracle rails.
