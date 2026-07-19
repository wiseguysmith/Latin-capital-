# 15 — Complaint & Dispute Handling (SOP)

| Field | Value |
|---|---|
| Purpose | Operationalize the dispute-routing matrix so each complaint reaches the responsible party — preventing capitalYA from accidentally owning regulated credit matters and preventing the lender from dumping everything on capitalYA |
| Audience | capitalYA/CRP ops & compliance, partner lender, counsel |
| Status | **DRAFT SOP — aligns with the partner agreement (`10`) and CRP `57`** |
| Version | 1.0.0 |
| Owner | Ops + Compliance |
| Dependencies | `06_...` §9 (routing matrix), `05_...`, CRP `57_COMPLAINT_CORRECTION_AND_APPEAL_PROCESS.md`, `36` |
| Last updated | 2026-07-19 |

---

## 1. Intake

All complaints (any channel) are logged with: complainant, date/time, channel, subject, and an initial category. **Acknowledged within 1 business day** (`06` §10). Every complaint is an audit event (CRP `36`).

## 2. Classification & routing (the matrix)

| Complaint type | Routed to (owner) | capitalYA role |
|---|---|---|
| Loan **decision** dispute (approve/decline/terms) | **Partner lender** | Route + record only — never re-decide |
| **Payment / balance** dispute | **Partner lender** | Display data, route |
| **CRP score** dispute | **CRP** (correction/appeal, CRP `57`) | Route to CRP appeal |
| **Data correction** request | **Data controller** for that data | Facilitate, log, forward |
| **Platform functionality** complaint | **capitalYA** | Own + resolve |
| **Fraud** allegation | **Joint escalation** | Co-investigate per agreement; AML SOP `14` if applicable |
| **Regulatory** complaint | **Responsible regulated party** (usually lender) | Cooperate, provide records |

Boundary rule: capitalYA **must not** accept responsibility for a regulated credit decision, and the lender **must not** casually reroute everything to capitalYA (`06` §9).

## 3. Handling & resolution

- Route within **1 business day** of classification; misrouted items re-routed with a logged reason.
- Owner investigates and responds within its SLA; capitalYA tracks status end-to-end for the borrower's visibility even when it is not the owner.
- **CRP score disputes** follow CRP `57`: access to submitted data, correction, **human review**, reassessment after remediation, model-version record.
- **Data-correction** requests follow the DSAR process (`13` §6).
- Outcomes recorded with reason codes; borrower informed of the result and any further recourse (incl. external/regulator channels where applicable).

## 4. Escalation & appeals

- Unresolved or contested outcomes escalate to the owner's senior reviewer; cross-party matters escalate to the capitalYA–lender governance contacts named in the agreement.
- CRP appeals have a defined final internal decision-maker (CRP `57`); capitalYA platform complaints escalate to the Head of Product/Ops.

## 5. Records, metrics & review

- Retain full complaint records (aligned to CRP `35`).
- Track volumes, categories, routing accuracy, time-to-acknowledge, time-to-resolve.
- Periodic review with the lender; systemic issues feed product/process fixes and, where methodology is implicated, CRP change control (`03_...`/CRP `20` §10).

## 6. Consumer-protection linkage

Complaint channels are **disclosed to borrowers** (`11` §2 item 13), in Spanish, with clarity on which party owns which complaint and how to reach external/regulator recourse (`03_...` §10.1).

## 7. Open items → agreement / counsel

`[Governance contacts + escalation ladder]`, `[external/regulator recourse references]`, `[resolution SLAs per owner]`, `[certified Spanish complaint notices]`.
