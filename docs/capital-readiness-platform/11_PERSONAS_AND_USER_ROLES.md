# 11 — Personas and User Roles

| Field | Value |
|---|---|
| Purpose | Canonical role registry with goals, permissions summary, and organization/account model |
| Audience | Product, design, engineering, operations |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | 12 (permissions detail), 13 (journeys) |
| Source references | Master prompt §9; CRF §13 (borrower/lender personas) |
| Assumptions | ASM-02 (small team wears multiple internal roles subject to segregation rules 12 §6) |
| Open questions | — |
| Approval required | Product owner |
| Last updated | 2026-07-16 |

## 1. Account model

- **Organization-based accounts.** Every user belongs to exactly one organization per membership; organizations are typed: `borrower`, `internal`, `capital_provider`, `partner`, `auditor`.
- A person may hold memberships in multiple organizations (e.g., a lawyer serving two partner firms) — separate memberships, separate role sets, no cross-visibility.
- **Deal-scoped access:** provider and partner access is granted per opportunity/workspace, never organization-wide by default (D-08).
- Invitations, suspension, removal, conflicts, segregation, and emergency access: 12 §5–7.

## 2. Role registry

### Borrower-side

| ID | Role | Who they are | Primary goals | Key permissions (summary; matrix in 12) |
|---|---|---|---|---|
| R-PA | Prospective applicant | Business owner/CFO pre-acceptance | Understand the process; submit preliminary application; track status | Create/edit own draft application; view own status |
| R-BA | Business administrator | Owner/CFO of accepted business; ≥1 required per borrower org | Complete readiness, manage team, respond to clarifications, consent to sharing, attest | Full workspace control; invite/remove R-BM; sign attestations; grant/revoke sharing consent |
| R-BM | Business team member | Accountant, ops manager, assistant | Upload documents, complete assigned tasks | Upload/edit assigned sections; no consent, attestation, member management, or capital-request submission rights |

### Internal platform

| ID | Role | Who they are | Primary goals | Key permissions |
|---|---|---|---|---|
| R-IA | Internal platform administrator | Ops lead | Run the platform: orgs, users, config, queues | Org/user admin; jurisdiction + scoring-model config (publish requires R-SA co-approval); all queues visibility |
| R-AR | Application reviewer | Intake analyst | Screen preliminary applications | Accept/reject/waitlist/RFI applicants; assign reviewers |
| R-FR | Financial reviewer | Credit-trained analyst | Verify financial evidence, propose A/B/G/H control maturities, resolve financial exceptions | Verify extractions; edit control assessments (dimensions A,B,D,G,H); raise clarifications/flags |
| R-LR | Legal reviewer | Lawyer (staff or supervised) | Verify legal evidence, propose C/E maturities, interpret documents | Verify legal docs; edit control assessments (C,E); raise flags; recommend legal-hold |
| R-CR | Compliance reviewer | AML/compliance analyst | KYC/KYB/UBO verification, sanctions/PEP/adverse-media disposition, F controls | Disposition screening hits; edit F controls; place compliance holds; restricted-flag visibility |
| R-SA | Senior approver | Senior credit/ops leader | Approve final assessments, overrides, publications, exceptions | Approve/return assessments; approve overrides; approve publication; approve scoring-model publish |
| R-SUP | Support user | Support staff | Help users without seeing sensitive data | View user/account metadata and statuses; no document, financial, or score content |

### Capital-provider side

| ID | Role | Who they are | Primary goals | Key permissions |
|---|---|---|---|---|
| R-CPA | Capital-provider administrator | Fund/bank team lead | Manage provider team, define interest, manage EOIs | Manage provider users; access authorized opportunities; submit RFI/EOI; manage internal notes |
| R-CPN | Capital-provider analyst | Analyst | Review packages, run own diligence | Read authorized opportunities/document rooms; draft RFIs/notes; cannot submit EOI |

### Partners & oversight

| ID | Role | Who they are | Primary goals | Key permissions |
|---|---|---|---|---|
| R-LP | Legal partner | External law firm | Provide legal review services on assigned workspaces | Deal-scoped read of assigned legal sections; upload opinions/reports; partner notes |
| R-CP | Compliance partner | External AML/KYC firm | Screening/verification services | Deal-scoped compliance data access; upload screening reports |
| R-AP | Accounting/financial-review partner | External accountant/auditor | Financial review/normalization services | Deal-scoped financial data access; upload review reports |
| R-AUD | Read-only auditor | External/internal auditor | Examine records and controls | Read-only across assigned scope incl. audit log; no PII exports without R-IA+R-SA approval; access itself audited |

## 3. Borrower personas (CRF §13.1, mapped to MVP borrower types D-16)

| Persona | Type mapping (24) | Readiness pains the product must address |
|---|---|---|
| Formalizing family SME | BT-SME | Personal/company cash mixing; weak reconciliation; undocumented related parties; succession |
| Growth-stage startup (rev-based/venture-debt candidate) | BT-STARTUP | No EBITDA; needs runway/cohort/investor-support evidence instead of forced SME metrics |
| Real-estate developer / project SPV | BT-RE-DEV | Title/permits/budget/draw controls; completion & takeout logic |
| Real-estate operating company | BT-RE-OP | Rent roll, NOI, leases, appraisal age, liens |
| Asset-heavy operator | BT-ASSET | Asset registers, maintenance, appraisals, WC pressure |
| Receivables-backed borrower | BT-AR | Receivables tape quality, dilution, debtor concentration, assignment rights |
| Project-finance-style opportunity | BT-PROJECT | Contracted cash flows, sources/uses, sponsor equity, milestone monitoring |

## 4. Capital-provider personas (CRF §13.2)

Relationship bank; private-credit fund; asset-based lender; venture-debt provider; family office; DFI/impact investor; fintech/embedded lender. MVP optimizes packaging for **private-credit funds and family offices** first (private-debt-first, D-01); bank-grade compliance evidence still captured so bank onboarding isn't blocked later.

## 5. Anti-personas (explicitly not served in MVP)

Consumers and sole proprietors (ASM-13); businesses seeking equity-only raises; crypto-asset issuers (81 future); borrowers outside CR until overlays activate; providers wanting the platform to make the credit decision for them (boundary 02).
