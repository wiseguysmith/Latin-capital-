# 05 — Decision Log

| Field | Value |
|---|---|
| Purpose | Authoritative register of resolved product decisions; second in the source-of-truth hierarchy |
| Audience | All |
| Status | Active log |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | 03 (hierarchy), 06 (open questions feed decisions) |
| Source references | Master prompt §4 (resolved decisions); CRF v1.0 |
| Approval required | Product owner for new entries; founder for boundary-affecting entries |
| Last updated | 2026-07-16 |

Status values: **Approved** (binding), **Proposed** (needs approval), **Superseded** (kept for history).

| ID | Date | Decision | Rationale / source | Status | Impacted docs |
|---|---|---|---|---|---|
| D-01 | 2026-07-16 | MVP is Costa Rica-first, private-debt-first, human-supervised, financing range ~USD $500k–$2M with architecture supporting larger; expansion via jurisdictional overlays to Panama and El Salvador | Master prompt §3 | Approved | 10, 60–63 |
| D-02 | 2026-07-16 | Businesses may start an application, but an administrator must manually approve entry to the full readiness process. Intake supports preliminary application, screening, acceptance, rejection, waitlisting, RFI-preliminary, reviewer assignment. Acceptance ≠ funding approval | Master prompt §4.1 | Approved | 13, 14, 15, 51 |
| D-03 | 2026-07-16 | Each approved applicant receives a capital workspace (profile, ownership, capital request, use of proceeds, preferences, checklist, document room + versions, tasks, deadlines, clarifications, results, remediation plan, communications, status, final package) | Master prompt §4.2 | Approved | 13, 17, 18 |
| D-04 | 2026-07-16 | Two-stage AI analysis: per-document after each upload; full-package after substantial completeness. Both route material findings to human verification | Master prompt §4.3 | Approved | 30, 32, 52 |
| D-05 | 2026-07-16 | Deterministic scoring per CRF: 8 dimensions totaling 100 (A15/B20/C12/D10/E13/F10/G14/H6), maturity 0–4, contribution = weight × maturity ÷ 4, bands per CRF §1, hard gates and caps per CRF §4.3 | Master prompt §5; CRF §4, Appendix A | Approved | 20, 21, 23 |
| D-06 | 2026-07-16 | Borrower can see the readiness score with band, evidence grade, dimension scores, strengths, missing evidence, blockers, conditions, recommended actions, and deltas since last assessment; with mandatory limitation statement | Master prompt §4.4 | Approved | 18, 27 |
| D-07 | 2026-07-16 | Human-in-the-loop is mandatory for: applicant approval, critical extraction verification, identity/UBO resolution, sanctions/adverse-media review, legal document interpretation, conflicting evidence, exceptions, overrides, final assessment approval, distribution approval, marketplace entry. All material human actions audited | Master prompt §4.5 | Approved | 12, 14, 23, 36, 50–56 |
| D-08 | 2026-07-16 | Capital providers get MVP accounts scoped to authorized opportunities only; workspace per prompt §4.6; providers perform their own underwriting downstream | Master prompt §4.6 | Approved | 12, 18, 54, 55 |
| D-09 | 2026-07-16 | Transaction boundaries per 02 §2 (no lending, custody, execution, tokenization, etc.) | Master prompt §4.7 | Approved | 02 |
| D-10 | 2026-07-16 | Tokenization: document compatibility (81) but keep off MVP critical path | Master prompt §4.8; CRF §20 | Approved | 81 |
| D-11 | 2026-07-16 | Evidence confidence stays separate from readiness at document, control, dimension, and assessment levels | Master prompt §5.4; CRF §4.2 | Approved | 22 |
| D-12 | 2026-07-16 | Red flags are not automatic rejections; severity model S1–S4 with defined resolvers, evidence, gate/cap/warning/condition effect, and per-audience visibility incl. legally-hidden flags | Master prompt §5.5 | Approved | 23 |
| D-13 | 2026-07-16 | AI allowed/prohibited matrix per prompt §6: assistive tasks allowed; approvals, sanctions clearance, legal/credit determinations, weight changes, gate overrides, publication decisions prohibited | Master prompt §6; CRF §16.1 | Approved | 31 |
| D-14 | 2026-07-16 | Documentation-first: no production application code until this suite is reviewed and approved | Master prompt §21 | Approved | 00 |
| D-15 | 2026-07-16 | Commercial model not finalized; MVP billing manual; fees disclosed; success fees (if ever) structurally separated from scoring; payment status can never affect readiness results | Master prompt §15 | Approved | 15, 78 |
| D-16 | 2026-07-16 | MVP borrower types: established SME, startup/emerging, real-estate developer, real-estate operating company, asset-heavy, receivables-backed, project-finance-style — with adaptive requirements by type/industry/maturity/financing/amount/use/jurisdiction/collateral/repayment | Master prompt §3, §11 | Approved | 24, 26 |
| D-17 | 2026-07-16 | Terminology: "capital provider" in product; CRF "lender" maps to it. "Readiness Report/Package" in MVP; "Certificate" post-pilot (REF-02) | 03 §3 | Approved | 04 |
| D-18 | 2026-07-16 | MVP publication is manual curation to named providers; automated matching deferred (REF-03) | D-07 conservatism | Approved | 55, 80 |
| D-19 | 2026-07-16 | Assessment validity 6 months; financial evidence ≤90 days at approval; identity/sanctions re-screen at approval and publication (ASM-07) | CRF Appendix B freshness | Proposed | 20, 22 |
| D-20 | 2026-07-16 | Scoring engine uses decimal arithmetic; rounding half-up to integer only at final displayed score (REF-01) | CRF Appendix A fractional weights | Approved | 20 |

## Change process

New decisions: open a PR editing this file; product owner approves; boundary-affecting decisions (02) additionally need founder + counsel. Superseding: mark old row Superseded, reference the new D-id.
