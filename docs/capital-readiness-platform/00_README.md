# 00 — README: Capital Readiness Platform Documentation System

| Field | Value |
|---|---|
| Purpose | Index, reading order, conventions, and executive handoff for the complete build-ready documentation suite |
| Audience | All roles: product, design, engineering, AI, operations, compliance, legal, capital providers |
| Status | Draft v1.0 — pending stakeholder review |
| Version | 1.0.0 |
| Owner | Head of Product (interim: founder) |
| Dependencies | All documents in this repository |
| Source references | `source/Capital_Readiness_Framework_Latin_America_v1.docx` (CRF v1.0, July 2026); Master Documentation Prompt (2026-07-16) |
| Approval required | Product owner sign-off on the suite as a whole |
| Last updated | 2026-07-16 |

## 1. What this platform is

A digital operating system that determines **how prepared a business is to receive institutional or private capital**, initially in Costa Rica (then Panama and El Salvador), for private-debt transactions of roughly USD $500k–$2M. The platform organizes, analyzes, verifies, scores, explains, and packages evidence so borrowers become more prepared and capital providers review opportunities more efficiently.

**The platform is not a lender, broker-dealer, custodian, or adviser.** It does not approve credit, guarantee funding, hold funds, execute loan agreements, or provide legal/tax/investment advice. See `02_PRODUCT_PRINCIPLES_AND_BOUNDARIES.md` — that document is binding on every other document and on all product copy.

## 2. Controlling sources

1. **Capital Readiness Framework (CRF) v1.0** — controlling source for readiness methodology (scoring dimensions, control library, maturity scale, bands, evidence confidence, gates/caps, external risk context). Archived at `source/`.
2. **Resolved MVP product decisions** — recorded in `05_DECISION_LOG.md` (D-01 through D-16).
3. Sound private-credit / security / AI-governance practice fills gaps; every gap-fill is an assumption labeled `ASM-xx` in `03_SOURCE_OF_TRUTH_AND_ASSUMPTIONS.md`.
4. Jurisdiction-specific statements are **not legal conclusions** until validated per `64_REGULATORY_AND_LEGAL_VALIDATION_CHECKLIST.md`.

## 3. Document map and reading order

| Series | Files | Start here if you are… |
|---|---|---|
| Foundation | 00–06 | anyone (read 01, 02 first) |
| Product | 10–19 | product manager, designer |
| Methodology | 20–28 | scoring/backend engineer, credit ops |
| AI & Data | 30–37 | AI engineer, data engineer |
| Technical architecture | 40–47 | backend/platform/security engineer |
| Operations & governance | 50–58 | operations, reviewers, compliance |
| Jurisdictional | 60–64 | legal counsel, compliance |
| Delivery | 70–79 | engineering leads, QA, program mgmt |
| Future | 80–82 | leadership, architects |

Recommended first read for engineers: `01 → 02 → 10 → 14 → 20 → 33 → 40 → 70`.
Recommended first read for operations: `01 → 02 → 11 → 13 → 50 → 51–56`.
Recommended first read for counsel: `02 → 60 → 64 → 06`.

## 4. Canonical identifier registries

All documents use these ID schemes. Never invent a parallel scheme.

| Prefix | Meaning | Defined in |
|---|---|---|
| `A1…H5` | CRF controls (48 controls, 8 dimensions) | 21 |
| `GATE-xx` | Eligibility & integrity gates | 23 |
| `CAP-xx` | Score caps | 23 |
| `FLAG severity S1–S4` | Red-flag severities | 23 |
| `EV-xxx` | Evidence types | 26 |
| `R-xx` | Roles | 11 |
| `FR-xxx-##` | Functional requirements | 15 |
| `NFR-##` | Non-functional requirements | 16 |
| `SCR-x##` | Screens | 18 |
| `ENT-##` | Data entities | 33 |
| `EVT (dot.namespaced)` | Audit events | 36 |
| `EP-## / US-###` | Epics / user stories | 70 |
| `TC-###` | Test cases | 73–74 |
| `D-##` | Decisions | 05 |
| `ASM-##` | Assumptions | 03 |
| `OQ-##` | Open questions | 06 |
| `RISK-##` | Risks | 78 |
| `CR-xx / PA-xx / SV-xx` | Jurisdiction rule items (Costa Rica / Panama / El Salvador) | 60–62 |

## 5. Non-negotiable invariants (enforced across all documents)

1. **Readiness ≠ underwriting.** The readiness score is never a credit score, PD estimate, approval, or funding promise (CRF §1, §21).
2. **Evidence confidence is separate from readiness** and is never blended into the 0–100 score (CRF §4.2).
3. **External risk context is reported, never a hidden score penalty** (CRF §8).
4. **Deterministic services own all math, gates, caps, permissions, and state transitions; generative AI explains and assists but never decides** (CRF §16).
5. **Humans approve** applicants, sanctions dispositions, legal interpretations, overrides, final assessments, and publication (D-07).
6. **Every material action is an audit event** (36).
7. **Jurisdiction logic lives in overlays**, never hardcoded (60–63).
8. **Payment/fee status must never influence readiness results** (15 §FR-SCORE; 78 RISK-14).

## 6. Executive handoff — how the build team should use this suite

1. **Week 1 (all leads):** read Foundation set; contest anything in `05_DECISION_LOG.md` now — after sign-off, decisions are binding until formally changed via the change process in `31_AI_GOVERNANCE_AND_MODEL_RISK.md` §9 / `50_ADMIN_OPERATING_MODEL.md` §7.
2. **Product/design:** build wireframes strictly from `18_SCREEN_BY_SCREEN_SPECIFICATION.md`; raise gaps as OQ entries, not ad-hoc design decisions.
3. **Backend:** implement the scoring engine from `20` + `21` + `23` exactly; the control library ships as versioned configuration, not code constants.
4. **AI engineering:** the permitted/prohibited matrix in `31` §3 is a hard boundary; extraction schemas in `32` are the contract with backend.
5. **Operations:** run Phase 0/1 manually using SOPs 51–56 before automation exists; SOPs are written to work with spreadsheets + the document vault alone.
6. **Legal/compliance:** work the checklist in `64`; nothing marked "requires counsel validation" may be treated as settled.
7. **QA:** derive test suites from `72` acceptance criteria and `73`/`74` plans; the traceability matrix `79` is the coverage authority.
8. **Any conflict between documents:** resolve in favor of the lower-numbered Foundation/Methodology document, log the conflict in `06_OPEN_QUESTIONS.md`, and fix the losing document — do not fork behavior in code.

**Do not begin production application code until this suite has been reviewed and approved by the product owner** (per the master prompt's execution instruction).

## 7. Change control for this documentation

- Docs are versioned semantically per file (header `Version`).
- Material methodology changes (weights, bands, gates, caps) require the approval chain in `20` §10 and a new scoring-model version.
- Every merged change updates `Last updated` and, where relevant, `05_DECISION_LOG.md`.
