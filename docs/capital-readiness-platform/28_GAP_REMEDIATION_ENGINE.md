# 28 — Gap-Remediation Engine

| Field | Value |
|---|---|
| Purpose | How the prioritized Readiness Action Plan is generated, prioritized, tracked, and kept honest (no promises) |
| Audience | Product, AI engineers, reviewers, borrower-success ops |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Framework owner + Head of Product |
| Dependencies | 20–26, 31 (AI limits), 58 (partner referrals) |
| Source references | CRF §1 (Readiness Action Plan), §14 stage 8 (remediation sprint), §16.1 (borrower coaching) |
| Assumptions | Impact estimates shown as ranges (conservatism beyond CRF) |
| Open questions | — |
| Approval required | Framework owner |
| Last updated | 2026-07-16 |

## 1. Purpose & posture

The Action Plan converts assessment results into specific work to reach the next readiness band (CRF §1). It is **coaching, not advice**: non-legal, non-investment, non-tax guidance with templates and partner referrals (CRF §16.1). It never promises a score outcome or funding.

## 2. Generation pipeline (deterministic skeleton + AI narrative)

```
1. Deterministic gap enumeration (engine output):
   - gates not passed → blocking actions
   - caps triggered → release actions
   - open flags S1–S3 → resolution actions
   - controls with maturity gap (current < 3) ranked by:
       priority_score = weight × (3 − maturity)/3 × feasibility_factor
   - confidence gaps: evidence upgradeable E1→E2/E3; stale/missing items
2. Feasibility factor (config per action template): quick-fix (documents exist,
   just missing) = 1.0; process-build (needs new practice) = 0.6;
   structural (governance/legal changes) = 0.3
3. Ordering: blockers first (gates/caps/S1-S2), then highest priority_score,
   then confidence upgrades; capped at 12 visible actions per plan
4. AI drafts plain-language action cards from templates (31 rules: citations,
   no invented facts); reviewer approves plan before borrower release (MVP)
```

## 3. Action card format

`{what to do, why it matters (control + plain language), evidence to produce (EV refs + spec), estimated effect: "typically affects <dimension> by up to X points" (range = weight × maturity delta ÷ 4, stated as ceiling, never a promise), effort tag (quick/process/structural), template links, partner referral option (58), status}`.

Honesty rules: effect language always "up to"; combined-plan projection shown only as "potential band if all completed" with caveat; caps/gates actions show band effect only after release conditions verified.

## 4. Templates & partner referrals

Template library (borrower-facing, ES/EN): debt schedule template (CRF §6.1 fields), 13-week cash flow, related-party register, UBO chart, use-of-proceeds/sources-uses, board-resolution samples (jurisdiction overlay provides local variants; marked "sample — obtain local counsel"). Referrals: accounting/legal/compliance partners per 58; referral ≠ endorsement; referral fees (if ever) disclosed and firewalled from scoring (D-15).

## 5. Tracking & reassessment loop

Actions become tasks (FR-WS-05); completion requires linked evidence reaching `verified`; plan progress on SCR-B13; "request reassessment" enabled when all blocking actions complete or borrower insists (with expectation-setting copy). Reassessment produces delta view (D-06). Metric: remediation conversion — share of borrowers moving up ≥1 band (CRF §22.1) — tracked in 37.

## 6. Guardrails

- No action may instruct outcome-shaping ("obtain a higher appraisal") — only evidence/process building; template list reviewed by compliance.
- Score-gaming watch: rapid resubmission patterns, document-theater indicators (new documents contradicting priors) feed anomaly review (CRF §21).
- Plans expire with their assessment; superseded plans retained in history.
