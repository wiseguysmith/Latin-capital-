# 10 — MVP Scope

| Field | Value |
|---|---|
| Purpose | Definitive in/out scope statement for the MVP (Phases 1–3), with phase mapping |
| Audience | Product, engineering, leadership |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | 05 (D-01…D-20), 75 (roadmap) |
| Source references | Master prompt §3–4, §16; CRF §22 |
| Assumptions | ASM-03 (language), ASM-10 (manual billing), ASM-13 (business-purpose only) |
| Open questions | OQ-01, OQ-10 |
| Approval required | Product owner |
| Last updated | 2026-07-16 |

## 1. MVP definition

The MVP is complete when: a Costa Rican business can apply; an admin can screen and accept it; the business can complete its workspace and evidence checklist; the platform (AI-assisted, human-verified) can produce a deterministic, approved readiness assessment with report and remediation plan; and an authorized capital provider can review the published package and respond with RFI/EOI — all with full audit trail and within the boundaries of 02.

## 2. In scope (MVP = Phases 1–3)

| Area | Included | Phase |
|---|---|---|
| Intake | Public landing, preliminary application, screening queue, accept/reject/waitlist/RFI-preliminary, reviewer assignment | 1 |
| Accounts | Org-based accounts, invitations, roles per 12, email+password with MFA, session mgmt | 1 |
| Workspace | Company profile, ownership/UBO capture, capital request, use of proceeds, financing preferences, personalized checklist, tasks, deadlines, status | 1 |
| Documents | Secure upload, versioning, document room, manual classification (P1) then AI classification (P2), review states, expiry/supersede | 1–2 |
| AI | Per-document pipeline (classify/extract/map/defects/clarifications/confidence), full-package analysis (reconciliation, contradictions, staleness, completeness), verification queues, draft explanations | 2 |
| Scoring | Deterministic engine: 48 controls, maturity capture, applicability profiles, caps/gates, bands, evidence confidence, external risk context capture, versioned model; manual maturity entry in P1 with AI-proposed maturities in P2 | 1–2 |
| Review & approval | Reviewer queues, control assessment UI, gate/flag review, override workflow, senior approval, borrower report generation | 1–2 |
| Remediation | Gap-remediation plan generation and tracking; reassessment | 2 |
| Providers | Provider onboarding (manual diligence), provider workspace, curated publication, approved document room, RFI, EOI, notes, statuses, access expiry | 3 |
| Audit & security | Immutable audit log, RBAC, encryption, signed URLs, malware scan, download tracking, consent ledger | 1 |
| Notifications | Email + in-app per 19 | 1–2 |
| Reporting | Borrower readiness report (PDF/HTML), internal reviewer report, provider package | 2–3 |
| Admin | Org/user management, application queue, assessment queue, scoring-model config (read + versioned publish), jurisdiction config (CR overlay), audit log viewer | 1–2 |
| Billing | Manual invoicing outside product; fee disclosure page | 1 |

## 3. Out of scope for MVP (explicit)

| Excluded | Why | Where documented |
|---|---|---|
| Lending, funds handling, payments processing | Boundary (02 §2) | 02 |
| Automated matching engine / marketplace ranking | D-18; human-curated publication instead | 80 |
| Tokenization features | D-10 | 81 |
| Open-banking, accounting/ERP, credit-bureau, e-registry API integrations | Phase 4; manual/document fallback per CRF §17.1 | 47, 75 |
| Panama / El Salvador activation | Phase 4 overlays; template built now | 61–62 |
| Automated KYC vendor integration | Phase 2+/4; manual protocol first (ASM-06) | 51, 47 |
| In-product billing/payments | ASM-10 | 15 §FR-ADM |
| Native mobile apps | Responsive web only | 16 |
| Public self-serve provider signup | Providers onboarded manually (54) | 54 |
| Secondary languages beyond ES/EN | ASM-03 | 17 |
| Consumer / sole-proprietor borrowers | ASM-13 legal conservatism | 60 |
| Portfolio monitoring / early-warning feeds | Post-MVP (CRF §14 stage 14) | 80 |

## 4. Scope guardrails

1. Any feature request that touches 02 §2 boundaries is rejected without founder+counsel decision.
2. Anything automatable but listed "manual" stays manual until its SOP has run ≥10 real cases (P9 manual-first).
3. Scope additions require a D-entry and backlog re-prioritization (71).

## 5. Financing scope at launch

Business-purpose private debt, ~USD $500k–$2M (soft bounds — admin may accept outside range with reason), Costa Rica jurisdictions, capital purposes per CRF §10: working capital, expansion, equipment, inventory, contract/receivables-backed, revenue-generating projects, bridge, real-estate development/operating, refinancing with credible repayment strategy. Financing-module coverage detail: 25.
