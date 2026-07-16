# 56 — Exception, Override, and Escalation SOP

| Field | Value |
|---|---|
| Purpose | Procedures for evidence exceptions, score/gate overrides, integrity investigations, and escalation paths |
| Audience | All internal reviewer roles, R-SA, compliance |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Framework owner + Compliance |
| Dependencies | 23 (model), 20 §4 (engine), 12 §6 (segregation), 36 (audit) |
| Source references | CRF §4.3 (overrides), §18 (human override, effective challenge); D-07, D-12; ASM-09 |
| Assumptions | ASM-09 |
| Open questions | — |
| Approval required | Framework owner + compliance |
| Last updated | 2026-07-16 |

## 1. Evidence exceptions (ENT-15)

Sources: reconciliation mismatches, contradictions, staleness, schema failures. Procedure: reviewer examines both sources → resolution options: (a) authoritative-source ruling (documented rationale — e.g., bank data over internal spreadsheet, CRF §17.1 conflicts create exceptions, signed/audited evidence not auto-outranked); (b) clarification to borrower; (c) flag if integrity-relevant. **Never average conflicting values** (CRF App B). Materiality per 21 §4 decides whether unresolved exception blocks `ready_for_final`. Accepted-immaterial exceptions are recorded with reason and visible in the internal report (27 §4).

## 2. Overrides (implements 23 §7)

1. **Propose** (R-FR/R-LR/R-CR): target (control maturity / exception acceptance / conditional-gate waiver / condition change), reason code, evidence, expiry (≤ assessment expiry).
2. **Validate** (system): proposer ≠ approver; band-movement ≤1 band (ASM-09 — beyond requires founder sign-off recorded as exceptional); prohibited targets rejected (weights, formulas, GATE-03/04 substance, sanctions dispositions).
3. **Approve/Reject** (R-SA, SLA ≤3 business days): documented note; approval applies at engine step 7; system score remains visible internally alongside published (CRF §4.3 "never silently overwrite").
4. **Monitor:** override register monthly (methodology board 50 §3): rate, direction, reviewer patterns, expiry outcomes; high rates = framework weakness → calibration proposal (CRF §22.1); analysts are never evaluated or compensated on score levels (D-15).
5. **Expiry:** lapse → recompute → notify; renewal = new proposal with fresh evidence.

## 3. Integrity investigations (GATE-04 protocol)

Trigger: suspected alteration (DEF-10), fabricated contracts, identity anomalies, manipulated statements, unexplained transactions (CRF §3.1). Procedure: suspend assessment (14 §3); restricted S1 flag (R-CR + R-SA co-sign; borrower messaging neutral — "additional verification required"); independent review (someone uninvolved in the workspace); evidence preservation (legal hold as needed, 35 §5); outcomes: (a) cleared — flag resolved with rationale, resume; (b) confirmed misrepresentation — reject/terminate relationship, retain records, counsel consult re obligations (64; e.g., CR Law 7786 reporting duties if registrable activity applies — counsel question, never self-determined); (c) inconclusive — enhanced-review conditions or termination per risk committee. **AI never labels fraud** (31 §3); it only surfaces signals.

## 4. Sanctions escalation (GATE-03)

Potential match → work stops on publication paths (assessment may continue collecting) → R-CR disposition with identifier comparison (AI may cluster/compare, never clear — CRF §16.1) → true match: reject/terminate + counsel consult re notification duties; false positive: documented disposition; uncertain: enhanced review + second reviewer (12 §6.5). All dispositions audited with rationale.

## 5. General escalation ladder

Reviewer → queue lead → R-IA/ops lead → R-SA → risk & compliance committee → founder+counsel. Anything touching regulatory perimeter (GATE-07), press/reputation, or law-enforcement contact goes straight to the committee. Escalations are tasks with SLAs — nothing escalates by hallway conversation alone (audit trail, D-07).
