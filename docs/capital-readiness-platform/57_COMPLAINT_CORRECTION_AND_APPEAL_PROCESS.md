# 57 — Complaint, Correction, and Appeal Process

| Field | Value |
|---|---|
| Purpose | Borrower/provider complaint handling, factual-correction path, and assessment appeals |
| Audience | Ops, borrower success, R-SA, compliance |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Ops lead |
| Dependencies | 53, 56, 37, 19 |
| Source references | CRF §18 (appeal/correction: borrowers can see missing items and reason codes, correct factual errors, resubmit), §19.2 (complaints process), §21 (bias/exclusion safeguard) |
| Assumptions | SLAs per 50 §5 (REF-08) |
| Open questions | Country consumer/SME complaint-handling duties (64 checklist item) |
| Approval required | Ops lead + counsel (external-facing text) |
| Last updated | 2026-07-16 |

## 1. Principles

Transparency by default: borrowers already see reason codes, missing evidence, and correction paths (D-06, CRF §18) — the appeal process exists for disagreement after that. No retaliation: filing never affects scores or queue treatment (D-15 discipline). Everything logged.

## 2. Channels & intake

In-product "raise an issue" (workspace/provider surfaces) + support email; R-SUP triages: **complaint** (service/conduct), **correction** (factual error in data/report), **appeal** (disagreement with assessment outcome/flag/condition), **privacy request** (routes to 44 §4 flow). Ack ≤2 business days.

## 3. Correction path

Factual error claims (wrong extracted value, wrong entity data, misattributed document): route to owning reviewer → verify against source → if error: correct with provenance, recompute (20 §6), reissue artifacts (27 §1 new version), notify affected providers if published (45 §5 score-integrity rules apply when the error changed results); if not error: explained response with evidence citation. SLA ≤10 business days.

## 4. Appeal path (assessment outcomes)

1. Borrower states the contested items (controls/flags/caps/conditions) + grounds + any new evidence.
2. **Independent re-review:** a reviewer not involved in the original assessment re-assesses contested items (small-team fallback: R-SA acts with declared original-involvement check; true independence gap logged — ASM-02).
3. Outcomes: uphold (reasoned response), adjust (recompute + reissue per correction path), or convert to reassessment (when new evidence is substantial).
4. R-SA signs the appeal outcome; one appeal per assessment per issue; further disagreement → reassessment cycle with new evidence.
SLA ≤15 business days.

## 5. Complaints (service/conduct)

Ops lead investigates; outcomes: explanation, apology+fix, SOP change proposal (50 §7); conduct issues re internal staff → founder; re provider conduct → 54 §4 (possible suspension).

## 6. Reporting & learning

Monthly: volumes, categories, SLA compliance, outcomes (37); appeal-driven adjustments feed methodology board (systematic anchor problems = calibration input, CRF §22.1); fairness lens: complaint/appeal patterns by segment reviewed quarterly (CRF §21 bias safeguard).
