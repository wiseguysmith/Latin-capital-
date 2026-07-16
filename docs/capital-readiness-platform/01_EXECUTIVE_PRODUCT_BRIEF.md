# 01 — Executive Product Brief

| Field | Value |
|---|---|
| Purpose | One-document orientation: what we are building, for whom, why, MVP shape, and success criteria |
| Audience | Leadership, investors in the platform company, all team leads |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | 02, 05, 10, 75 |
| Source references | CRF v1.0 §1–3, §22; Decision log D-01–D-16 |
| Assumptions | ASM-01 (working name), ASM-02 (team size) |
| Open questions | OQ-01 (commercial model), OQ-02 (entity/licensing structure) |
| Approval required | Founder/CEO |
| Last updated | 2026-07-16 |

## 1. The problem

Private capital in Latin America loses time and money on unprepared borrowers. Businesses seeking $500k–$2M in Costa Rica, Panama, and El Salvador typically present fragmented, unreconciled, stale, or unverifiable information. Lenders repeat the same document chase on every deal; borrowers don't know what "prepared" means; good businesses get slow answers and weak businesses consume diligence budgets. There is no shared, auditable standard for "this opportunity is organized, evidenced, and reviewable."

## 2. The product

A **capital-readiness operating system** implementing the Capital Readiness Framework (CRF):

- **Borrowers** get a guided capital workspace: adaptive document checklist, secure document room, per-document AI feedback, clarification workflow, a 0–100 Capital Readiness Score with dimension detail, an Evidence Confidence grade (A–D), red flags/conditions, and a prioritized gap-remediation plan.
- **Internal reviewers** get queues, verification workflows, deterministic scoring with human approval, override governance, and a full audit trail.
- **Capital providers** get standardized, consented, pre-organized opportunity packages: readiness summary, evidence-confidence grade, approved document room, request-for-information and expression-of-interest workflows.

The long-term ambition is a **SOC 2 / ISO-style standard for capital readiness** — methodology + evidence + controls + human review + auditability — with the software as its operating system.

## 3. What the platform is not (hard boundaries)

No lending, no fund custody or transmission, no underwriting decisions, no credit approval, no investment recommendations, no guarantees of funding, no legal/tax/accounting/investment advice, no automated execution of loan agreements, no tokenization in MVP. Capital providers always perform their own underwriting downstream. Full boundary list: `02_PRODUCT_PRINCIPLES_AND_BOUNDARIES.md`.

## 4. Five assessment outputs (CRF §1)

| Output | Meaning | Explicitly not |
|---|---|---|
| Capital Readiness Score (0–100) | Weighted preparedness of borrower + deal | PD, rating, approval, pricing |
| Evidence Confidence (A–D) | Strength/independence/freshness of evidence | Audit, appraisal, legal opinion |
| External Risk Context (1–5) | Country/industry/currency/climate exposures | A hidden score penalty |
| Red-Flag & Conditions Register | Issues needing rejection, escalation, remediation, or review | An automated adverse decision |
| Readiness Action Plan | Work to reach the next band | A promise of financing |

## 5. MVP shape (Costa Rica-first, private-debt-first)

- Manual admin acceptance of applicants (D-02) → capital workspace → document collection with two-stage AI analysis (per-document + full-package, D-04) → human verification → deterministic scoring → senior approval → optional publication to authorized capital providers (D-08).
- Human-supervised at every material decision (D-07). Institutionally credible: audit log, versioned scoring model, override governance.
- Expandable to Panama/El Salvador via jurisdictional overlays (60–63); architecture supports >$2M transactions and future tokenization compatibility without MVP dependency (80–81).

## 6. Users

Borrower organizations (established SMEs, startups with credible revenue/asset-based opportunities, real-estate developers/operators, asset-heavy and receivables-backed businesses); internal platform team (application, financial, legal, compliance reviewers; senior approvers); capital providers (private lenders, banks, family offices, venture-debt, private-credit funds, institutional investors); partners (legal, compliance, accounting); auditors. Full persona set: `11_PERSONAS_AND_USER_ROLES.md`.

## 7. Why we win

1. **Standard, not marketplace-first:** the defensible asset is the certified methodology + evidence discipline, not a listing site.
2. **Borrower coaching loop:** remediation plans convert "not ready" businesses into future pipeline instead of rejections.
3. **Institution-grade governance from day one:** deterministic scoring, evidence provenance, override discipline — credible to banks, funds, and future regulators.
4. **Jurisdictional overlay architecture:** each new country is configuration + counsel validation, not a rebuild.

## 8. Delivery phases (summary; detail in 75)

| Phase | Content | Exit test |
|---|---|---|
| 0 | Validate CRF controls, country assumptions, document matrix, reviewer workflow; manual dry-runs | 3–5 manual assessments completed end-to-end |
| 1 | Internal-operations MVP: intake, manual approval, workspace, uploads, internal review, manual scoring support, audit log, borrower report | 10 real applicants processed in-product |
| 2 | AI-assisted MVP: classification, extraction, evidence mapping, clarifications, contradiction detection, provisional scoring, verification queues | AI outputs verified faster than manual baseline |
| 3 | Capital-provider workspace: onboarding, curated opportunities, document sharing, RFI, EOI, tracking | First provider reviews a live package |
| 4 | Overlays & integrations: Panama, El Salvador, registry checks, KYC/AML providers, accounting integrations | Second country live under counsel sign-off |

## 9. Success metrics (pilot; from CRF §22.1)

Median time-to-complete readiness package; first-pass completeness rate; reconciliation exception rate and resolution time; reduction in repetitive lender information requests; borrower remediation conversion (share moving up ≥1 band); override rate/direction; provider time-to-screening-decision; fairness/access patterns across lawful segments.

## 10. Top risks (summary; register in 78)

Readiness mistaken for creditworthiness (mitigation: disclaimers, language controls, provider attestation); regulatory perimeter creep (counsel memos before each country/product activation); score gaming (reconciliation + attestation + sampling); AI hallucination (deterministic core, citation requirements, verification queues); conflicts from fees (score governance independent of billing, D-15).
