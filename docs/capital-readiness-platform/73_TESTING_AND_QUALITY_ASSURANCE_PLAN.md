# 73 — Testing and Quality Assurance Plan

| Field | Value |
|---|---|
| Purpose | Test strategy, suites, environments, and quality gates for the MVP |
| Audience | QA, engineering |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | QA lead |
| Dependencies | 72 (ACs), 74 (security/AI plan), 20 §11 (engine tests), 46 (CI) |
| Source references | Master prompt §20 (QA builds tests from ACs) |
| Assumptions | — |
| Open questions | — |
| Approval required | QA + engineering leads |
| Last updated | 2026-07-16 |

## 1. Strategy

Test pyramid with two special spines: (a) **deterministic-scoring spine** — golden vectors and reproducibility tests run on every commit; (b) **permission/audit spine** — matrix-generated authorization + audit-event tests run on every commit. E2E covers the four journeys (13 J1/J2/J5/J6). ACs in 72 are the source of truth; every TC references an AC/FR (79).

## 2. Suites

| Suite | Contents | Gate |
|---|---|---|
| Unit | modules, validators, comparators, metric derivations (34 §3 formulas each have fixture tests) | PR |
| Scoring golden (TC-SCORE-01…08) | per-profile vectors, caps, N/A, overrides, rounding edges, context isolation, reproducibility across deploys | PR + nightly cross-version replay |
| Permission matrix (TC-SEC-01…) | generated from 12 §2: every role × resource × action; tenancy isolation; segregation combinations (12 §6) | PR |
| Workflow (TC-WF-*) | every legal transition (14 §2–5) + representative illegal ones per lifecycle | PR |
| Audit (TC-AUD-*) | event emission per mutation; chain verification; export dual-approval | PR + daily prod verification job |
| Integration | API contracts (OpenAPI-validated), outbox delivery, queue idempotency, signed-URL issuance | merge |
| E2E (staging) | J1 borrower to approved result; J5 internal production; J6 publication; J2 provider RFI/EOI; appeal/correction flow (57) | release |
| Artifact tests (AC-RPT) | render determinism, audience filters, hash recording, PDF a11y | release |
| Performance | NFR-01…04 load tests at 10× pilot profile | pre-pilot + quarterly |
| Accessibility | WCAG 2.1 AA automated (axe) + manual screen-reader pass on borrower surfaces | release |
| i18n | string coverage, locale rendering, legal-text version checks | PR (lint) + release |
| Data lifecycle (TC-RET/TC-CONF) | retention execution + destruction certificates, legal-hold override, config hot-reload + version pinning | release |
| Resilience | degradation matrix drills (45 §4): vendor-down → manual queues; audit-store-down → fail closed | pre-pilot |

## 3. Test data

Synthetic borrower fixtures per business type (24 §6 profiles) incl. Spanish-language documents, poor scans, contradictory pairs (for exception tests), and a full "golden borrower" whose correct score is hand-computed (matches 20 §5). **No production data in non-prod ever** (SB-08); fixture generator maintained as code.

## 4. Defect policy

Sev-1 (cross-tenant, score-integrity, restricted-leak, audit-integrity, GATE bypass): stop-ship, incident process (45 §5). Sev-2 (wrong results with workaround, SLA breach class): fix before next release. Sev-3/4 scheduled. Any Sev-1/2 in scoring/permissions adds a permanent regression test.

## 5. UAT & pilot rehearsal

Phase-gate UAT with ops running real SOPs (51–56) on staging fixtures; pilot rehearsal = full dry-run of 76 §3 script before first real borrower. Reviewer sign-off recorded.

## 6. Copy & claims lint (02 §3)

CI job scans frontend strings, templates, and report templates for prohibited terms ("pre-approved", "guaranteed", "credit score", "investment grade", "safe investment", ES equivalents). Failures block merge. Allowlist requires compliance sign-off. Non-equivalence statements (04 §end) covered by dedicated copy tests on score/report surfaces.
