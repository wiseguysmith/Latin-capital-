# 53 — Readiness Assessment Approval SOP

| Field | Value |
|---|---|
| Purpose | Procedure from control assessment through senior approval and result release |
| Audience | R-FR, R-LR, R-CR, R-SA |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Framework owner + Ops lead |
| Dependencies | 14 §3, 20–23, 27, 52 |
| Source references | CRF §14 stages 6–10; D-06, D-07 |
| Assumptions | ASM-16 |
| Open questions | — |
| Approval required | Framework owner |
| Last updated | 2026-07-16 |

## 1. Preconditions to enter control assessment

Checklist coverage threshold met (default 85% required items) or reviewer-triggered; full-package analysis complete (Phase 2) or manual reconciliation worksheet complete (Phase 1); GATE-05/06 statuses current.

## 2. Dimension assessment (per reviewer ownership 21 §5)

1. For each active control: review linked evidence (verified only), AI-suggested maturity + rationale (Phase 2, advisory), set maturity 0–4 against anchors (21 §2) **with evidence citations**; N/A only per profile/taxonomy (24 §4).
2. Raise flags per trigger table (21) — severity per 23 §4; raise clarifications for gaps that look resolvable.
3. Confirm dimension confidence inputs (22): tiers, freshness, contradictions cleared.
4. **Section sign-off** per dimension group (audit: section.signed). All sections signed + gates dispositioned + exceptions resolved/accepted → `ready_for_final`.

## 3. External Risk Context entry (FR-SCORE-05)

R-FR proposes level 1–5 + factors + mitigants from the CR context library (60 §6) and borrower-specific facts; R-SA confirms at final review. Never blended into score (test-enforced).

## 4. Final review (R-SA; SLA ≤5 business days)

Review package: score (system + any override-published), band, caps triggered w/ pre-cap values, confidence grade + computation summary, gate board, full flag register (incl. restricted), overrides pending/applied, exceptions history, reviewer sign-offs (segregation validated automatically), External Risk Context, draft borrower report + action plan (AI-drafted, cited).

Decisions:
| Decision | When | Effect |
|---|---|---|
| Approve | no unresolved gates/S1-S2; confidence ≥ B; conditions unnecessary | result released to borrower (D-06); report generated (27 §3) |
| Approve with conditions | resolvable issues; confidence C (22 §7); S3 flags | conditions enumerated, visible, tracked |
| Return | specific deficiencies | back to named queue with notes |
| Not ready | material gaps; borrower gets full plan | result + remediation plan released; band as computed |

R-SA may not approve an assessment where they proposed any control maturity or override (12 §6).

## 5. Release & post-approval

Report + action plan released (SCR-B10…B15); validity clock starts (6 months, ASM-07); attestation flow available; borrower notified (RES-01/02/03 — no score value in email). Post-approval changes → material-change rules (20 §9).

## 6. Reassessment

Same procedure; deltas computed vs last approved result (D-06); expedited path allowed when only named gaps changed (reviewers re-assess affected controls + re-run package analysis; full gate re-check always).
