# 50 — Admin Operating Model

| Field | Value |
|---|---|
| Purpose | How the internal team runs the platform: org design, cadences, queue SLAs, escalation, and governance forums |
| Audience | Operations, leadership |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Ops lead |
| Dependencies | 11/12 (roles), 51–57 (SOPs), 37 (metrics) |
| Source references | CRF §18; D-07; ASM-02 |
| Assumptions | ASM-02 (pilot team size — people hold multiple roles under 12 §6 constraints) |
| Open questions | — |
| Approval required | Ops lead + founder |
| Last updated | 2026-07-16 |

## 1. Operating principles

Manual-first (P9); every material action audited (D-07); segregation over speed (12 §6); SLAs published to users (13 guardrails); SOPs 51–57 are the executable procedures — this document is the frame around them.

## 2. Team & role mapping (pilot, ASM-02)

| Function | Roles held | Segregation notes |
|---|---|---|
| Ops lead | R-IA (+R-AR backup) | never R-SA on same assessment they screened |
| Credit analyst(s) | R-FR (+R-AR) | cannot senior-approve own work |
| Legal (staff/fractional) | R-LR | interprets docs; flags to counsel per 64 |
| Compliance (fractional→hire) | R-CR | sanctions dispositions dual-controlled |
| Senior approver | R-SA (founder or senior credit hire) | approves assessments, overrides, publications, model publishes |
| Borrower success | R-SUP + task nudging | no reviewer roles (D-15 firewall from anything score-adjacent) |

## 3. Cadences

- **Daily:** queue standup (15 min) — SLA breaches, blocked assessments, escalations.
- **Weekly:** pipeline review (applications → assessments → opportunities), provider activity, AI exception trends.
- **Monthly:** methodology board — override register (23 §7), cap/flag stats, calibration items (OQ-15), model-change proposals (20 §10); restricted-flag review (23 §6); privileged-access sampling (SB-11).
- **Quarterly:** access reviews (SB-04), legal-hold review (35 §5), fairness report (37), risk-register refresh (78), DR/backup drills (45 §6).

## 4. Governance forums

| Forum | Members | Authority |
|---|---|---|
| Methodology board | framework owner, R-SA, R-FR rep, compliance | scoring model changes (20 §10), applicability profiles, calibration |
| Risk & compliance committee | founder, compliance, counsel (as needed), ops lead | perimeter questions (GATE-07/64), restricted flags policy, incident outcomes |
| Change advisory (lightweight) | eng lead, product, ops | release/readiness for feature launches touching SOPs |

## 5. Queue SLAs (published where user-facing)

| Queue | SLA | Escalation |
|---|---|---|
| Application screening (51) | decision or RFI ≤5 business days | R-IA at T+3, founder at T+7 |
| Document verification (52) | ≤3 business days per document | queue lead |
| Clarification response review | ≤2 business days after borrower answer | queue lead |
| AI exceptions | ≤2 business days | AI lead if systemic |
| Final assessment review (53) | ≤5 business days from ready_for_final | R-SA calendar guard |
| Override decision (56) | ≤3 business days | R-SA |
| Publication approval (55) | ≤3 business days | R-SA |
| RFI routing | ≤1 business day to route | ops lead |
| Complaints/appeals (57) | ack ≤2, resolution ≤15 business days | founder |

## 6. Capacity & workload

Pilot planning number: one R-FR can carry ~6–8 active assessments; document verification ~30–40 docs/day/reviewer (validate in Phase 0 dry-runs, 76). Queue dashboards (37) drive hiring triggers: sustained >80% capacity for 4 weeks → hire.

## 7. SOP change control

SOPs versioned in this repo; changes PR-reviewed by ops lead + affected role leads; changes touching methodology/boundaries also via methodology board / risk committee; every SOP change logged in 05 if it alters a decision.
