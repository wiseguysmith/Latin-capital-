# 72 — Acceptance Criteria

| Field | Value |
|---|---|
| Purpose | Canonical acceptance criteria: global gherkin-style criteria for every FR/US/screen family, with fully-expanded exemplars per pattern |
| Audience | QA, engineering, product |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | QA lead + Head of Product |
| Dependencies | 15 (FRs), 18 (screens), 70 (stories), 73/74 (test plans) |
| Source references | Master prompt §8 (testable requirements), §17 (ACs mandatory) |
| Assumptions | — |
| Open questions | — |
| Approval required | QA + product |
| Last updated | 2026-07-16 |

## 1. Global acceptance criteria (apply to every story; verified once per surface + regression)

- **AC-G1 Permissions:** every endpoint/action 403s for unlisted roles per 12 §2 (matrix-generated tests); UI never renders actions the API would deny.
- **AC-G2 Audit:** every mutation emits its named event (36 §2) with actor/object/context; missing event = failing test.
- **AC-G3 States:** only transitions in 14 succeed; invalid ones 409 with machine-readable blockers + `workflow.invalid_transition_attempted` logged.
- **AC-G4 Limitation statement:** any response/screen/artifact containing score or confidence includes the statement component/object (02 §3); automated check on API payloads and rendered artifacts.
- **AC-G5 i18n:** all user-visible strings resolve in ES and EN; no hardcoded literals (lint).
- **AC-G6 States of screens:** empty/loading/error/warning per 18 §1 present for every listed screen (storybook snapshot review).
- **AC-G7 Tenancy:** cross-org access attempts fail at API and RLS layers (TC-SEC-01 suite).
- **AC-G8 Idempotency:** repeating any POST with same Idempotency-Key does not duplicate effects.

## 2. Exemplar expanded criteria (patterns; every sibling story adopts the same shape)

### AC-US-201 Preliminary application (pattern: form capture)
```gherkin
Given an anonymous visitor with a verified email
When they complete all required fields and consent checkboxes and submit
Then applicant state = submitted, applicant.submitted emitted,
  ack notification APP-01 sent, and the screening queue shows the item ≤1 min
Given a saved draft older than 30 days When opened Then it resumes with data intact
Given missing consent When submitting Then submit is blocked with field-level error
Given a duplicate legal name + country When submitted Then submission succeeds
  and the screening view shows a duplicate-match panel (soft match, no hard block)
```

### AC-US-303 Capital request (pattern: validated business object)
```gherkin
Given UoP line items summing to ≠ amount ±1% Then submission is blocked with the delta shown
Given no repayment source Then a CAP-02 explanation renders and submission is blocked
Given an approved assessment exists When amount changes >10% Then material-change
  evaluation runs and assessment.reassessment_required may fire (20 §9 rules)
Given opportunity published When editing request Then edit requires amendment flow (R-SA)
```

### AC-US-601 Scoring engine (pattern: deterministic computation)
```gherkin
Given golden vector inputs for profile P-SME-WC-UNSEC (20 §5)
Then compute() returns exactly the documented totals, dimensions, and band
Given identical AssessmentInput and model version across 100 runs and 2 deploys
Then results are byte-identical (input_hash equal, output equal)
Given CAP-01 trigger active Then published_total ≤ 59 and caps_triggered records pre/post
Given External Risk Context changed from 1 to 5 Then score output is unchanged
Given a control maturity set without evidence refs on a material control
Then the API returns 422 and no recompute occurs
Given an N/A on a control not permitted by the profile Then 409 na_not_allowed
```

### AC-US-606 Overrides (pattern: governed human decision)
```gherkin
Given an override proposed by reviewer X When X attempts approval Then 403 segregation_conflict
Given an approved override moving band by 2 Without founder approval Then rejected (ASM-09)
Given an approved override Then result shows system_total and published_total, both retained
Given override expiry passes Then engine recomputes and override.expired is emitted
Given a proposed override targeting a weight or GATE-03 disposition Then rejected at validation
```

### AC-US-802 Packaging + disclosure lint (pattern: filtered publication)
```gherkin
Given a package containing a restricted flag reference When lint runs Then publish is blocked
  and the violation names the exact field
Given an S2 flag affecting the published result not in the disclosure set Then publish blocked
Given a clean package with recorded consents for providers A,B
When R-SA approves Then grants exist for A,B only, with expiry = 90d default,
  opportunity.published emitted, PROV-01 sent to A,B
Given provider C (no consent) Then C sees nothing and any direct fetch 403s + audits
```

### AC-US-1001 Consent revocation (pattern: propagation SLA)
```gherkin
Given borrower revokes consent for provider A at T0
Then A's grant is revoked and access fails by T0+1h (NFR-09), access.revoked emitted,
  A notified, audit retains historical access records (35)
```

### AC-SCR-B10 Score screen (pattern: results surface)
```gherkin
Given an assessment in approved state Then borrower sees score, band+definition,
  confidence grade+reason, 8 dimension bars, strengths, blockers, conditions,
  validity date, delta panel (if prior), External Risk Context panel labeled
  "not part of your score", and the limitation statement (non-dismissable)
Given assessment in any pre-approval state Then no score is shown (ASM-16), status instead
Given an expired assessment Then an expired badge + renewal CTA render, provider views pause
Given a restricted flag exists Then nothing on this screen indicates its existence
```

### AC-RPT-01 Report artifacts (pattern: rendered artifact)
```gherkin
Given the same AssessmentResult When rendering twice Then artifacts are identical and
  hash-recorded; borrower/internal/provider variants each pass their audience filter tests
  (restricted content in borrower/provider variant = release-blocking Sev-1)
```

## 3. Coverage rule

Every US in 70 must have either (a) an expanded AC block here, or (b) an adopted pattern reference (`pattern: X + story-specific deltas`) recorded in the story's traceability row (79) before implementation starts. QA owns enforcement (73 §2).
