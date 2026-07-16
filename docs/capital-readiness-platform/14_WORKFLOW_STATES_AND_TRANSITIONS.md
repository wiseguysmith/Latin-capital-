# 14 — Workflow States and Transitions

| Field | Value |
|---|---|
| Purpose | Complete state machines for applicant, assessment, document, and opportunity lifecycles; the deterministic workflow contract for engineering |
| Audience | Backend engineering, QA, operations |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product + Backend lead |
| Dependencies | 12 (actors), 36 (events), 15 (requirements) |
| Source references | Master prompt §10; CRF §14–15 |
| Assumptions | ASM-07 (expiry values) |
| Open questions | — |
| Approval required | Product owner |
| Last updated | 2026-07-16 |

## 1. Conventions

- State machines are enforced by the deterministic workflow service (41); no AI may execute a transition (P3).
- Every transition emits an audit event named `<entity>.<transition>` (36) recording actor, precondition evidence, and prior state.
- **Reversal rules**: states are never mutated backward silently; corrections happen via explicit reverse transitions listed below, preserving history.
- All data in all states is retained per 35 unless a transition row says otherwise; deletion is only via retention policy or approved erasure (35 §5).
- Notifications per transition: see 19 §4 (matrix keyed by the same event names).

## 2. Applicant lifecycle

States: `draft` → `submitted` → `under_preliminary_review` → (`info_requested` ⇄) → `accepted` | `waitlisted` | `rejected`; `withdrawn` from any pre-decision state.

| Transition | Actor | Preconditions | Required evidence/inputs | Blocking conditions | Notifications | Audit event | Reversal | External visibility |
|---|---|---|---|---|---|---|---|---|
| create draft | R-PA | consent checkbox (processing) | contact email verified | — | — | applicant.draft_created | delete own draft | applicant only |
| submit | R-PA | all required fields; consent to screening | preliminary form | prohibited-activity self-declared → warn, still submits | ack to applicant | applicant.submitted | withdraw | applicant |
| start review | R-AR | submitted | queue assignment | — | — | applicant.review_started | — | applicant sees "under review" |
| request info | R-AR | under review | question list | — | applicant notified | applicant.info_requested | answer returns to under review | applicant |
| accept | R-AR (R-SA required if outside amount range/sector list) | screening checklist complete (51); no unresolved S1 preliminary flag | screening record | unresolved sanctions concern; duplicate org | acceptance email (explicit not-a-funding-approval wording) | applicant.accepted | revoke-acceptance (R-SA, reason; only before workspace activity) | applicant |
| waitlist | R-AR | under review | reason code | — | waitlist email | applicant.waitlisted | promote to accepted / reject | applicant |
| reject | R-AR | under review | reason code (taxonomy 51 §7) | — | rejection email w/ reapply guidance | applicant.rejected | R-SA may reopen ≤30 days | applicant |
| withdraw | R-PA | any pre-decision | — | — | confirm | applicant.withdrawn | reapply later (new record) | applicant |

Data retained on reject/withdraw: preliminary form + screening record per retention 35 (compliance basis); documents beyond preliminary form are not collected pre-acceptance.

## 3. Readiness assessment lifecycle

States: `workspace_setup` → `collecting_evidence` → `ai_analysis_in_progress` → `human_review_required` → (`clarification_requested` ⇄ `collecting_evidence`) → (`remediation_in_progress` ⇄) → `ready_for_final_assessment` → `final_review` → `approved` | `approved_with_conditions` | `not_ready`; plus `expired`, `reassessment_required`, `suspended` (org suspension 12 §5). Sub-flag: `preliminary` when GATE-06 minimum dataset unmet (REF-05).

| Transition | Actor | Preconditions | Blocking conditions | Audit event | Reversal | Borrower visibility |
|---|---|---|---|---|---|---|
| activate workspace | system on applicant.accepted | borrower org + R-BA exist | — | assessment.workspace_activated | — | full |
| begin collecting | R-BA completes profile/request | checklist generated | — | assessment.collection_started | — | full |
| run full-package analysis | system (threshold: required checklist items ≥ config %, default 85%) or R-FR manual trigger | per-doc pipelines settled | pipeline failures → retry policy 30 §8 | assessment.package_analysis_started | — | status only |
| require human review | system | analysis complete | — | assessment.review_required | — | status only |
| request clarification | R-FR/R-LR/R-CR | approved clarification items | — | assessment.clarification_requested | resolved by borrower response | question content visible |
| enter remediation | R-FR or system (post-approval plan) | gaps identified | — | assessment.remediation_started | — | full plan |
| ready for final | dimension reviewers all sign section-complete | all controls assessed or N/A; gates dispositioned; exceptions resolved/accepted | open S1 flag; unmet GATE-01…07 | assessment.ready_for_final | return by R-SA | status only |
| final review | R-SA | package complete | approver = any proposer (blocked, 12 §6) | assessment.final_review_started | — | status only |
| approve / approve-with-conditions | R-SA | caps applied; confidence grade computed; conditions enumerated | GATE failures; CAP-03-class unresolved | assessment.approved | supersede via reassessment only | full results (D-06) |
| mark not-ready | R-SA | reasoned decision + remediation plan attached | — | assessment.marked_not_ready | new assessment cycle | full results + plan |
| expire | system (validity ASM-07: 6 months, or material-change trigger, or evidence staleness rule) | — | — | assessment.expired | reassessment | badge "expired" everywhere incl. provider views |
| require reassessment | system/R-SA (material change reported, flag raised, evidence superseded) | — | — | assessment.reassessment_required | — | status + reason category |
| suspend | R-IA | org suspension/compliance hold | — | assessment.suspended | unsuspend R-IA+reason | status only |

## 4. Document lifecycle

States: `requested` → `uploaded` → `processing` → `classified` → `extraction_completed` → `human_verification_required` → `verified` | `rejected`; plus `superseded`, `expired`, `deleted_under_policy`, `legally_preserved`.

| Transition | Actor | Preconditions | Blocking | Audit event | Reversal | Visibility notes |
|---|---|---|---|---|---|---|
| request | system (checklist) or reviewer | checklist rule / clarification | — | document.requested | cancel request | borrower sees request + spec (26) |
| upload | R-BA/R-BM (or partner†) | malware scan pass; format/size per ASM-11 | scan fail → quarantined, notified | document.uploaded | — | uploader org |
| process | system | queued pipeline | pipeline failure → retry/backoff then manual queue | document.processing_started | — | status |
| classify | AI (P2) / reviewer (P1) | — | low confidence → verification required | document.classified | reclassify (reviewer, reason) | internal |
| complete extraction | AI | classification | schema mismatch → exception | document.extraction_completed | re-extract on new model version (versioned) | internal |
| require verification | system (rules: material fields, low confidence, contradictions) | — | — | document.verification_required | — | internal |
| verify | R-FR/R-LR/R-CR per class | checks per 52 | — | document.verified | un-verify (R-SA, reason) | borrower sees "accepted" |
| reject | reviewer | reason (defect taxonomy 26 §5) | — | document.rejected | new version upload | borrower sees reason + fix guidance |
| supersede | system on new verified version | newer version verified | — | document.superseded | — | old versions retained, marked |
| expire | system (max-age rule per evidence type 26) | — | — | document.expired | replacement upload | borrower prompted |
| delete under policy | system (retention 35) | retention elapsed; no legal hold | legal hold | document.deleted_under_policy | none (destruction certificate) | — |
| legally preserve | R-IA/R-CR (hold) | hold order recorded | — | document.legal_hold_applied | hold release (R-SA) | internal only |

## 5. Opportunity lifecycle

States: `internal_only` → `packaging` → `pending_publication_approval` → `approved_for_selected_providers` → `published` → (`information_requested` ⇄, `interest_expressed` ⇄) → `in_external_underwriting` → `funded_externally` | `declined_externally`; plus `paused`, `closed`, `withdrawn` (borrower), `archived`.

| Transition | Actor | Preconditions | Blocking | Audit event | Reversal | Visibility |
|---|---|---|---|---|---|---|
| create (internal_only) | R-IA | assessment `approved`/`approved_with_conditions` | expired assessment | opportunity.created | delete before publication | internal |
| package | R-IA | disclosure set chosen (23 §6); package artifacts generated (27 §5) | restricted flags leaking → blocked by lint | opportunity.packaged | repackage | internal |
| submit for publication approval | R-IA | borrower consent recorded for named providers | missing consent | opportunity.publication_requested | withdraw request | internal |
| approve for selected providers | R-SA | consent + package + fresh sanctions re-screen (ASM-07) | stale assessment; open S1/S2 undisclosed | opportunity.publication_approved | revoke (R-SA) | internal |
| publish | system | approval + provider grants configured (expiry ASM-12) | — | opportunity.published | unpublish (R-SA/R-IA) | named providers see it |
| record RFI | R-CPA/R-CPN(draft) | published + access valid | — | opportunity.rfi_submitted | withdraw RFI | borrower + internal |
| record EOI | R-CPA | published; provider agreement active | — | opportunity.eoi_submitted | withdraw EOI | borrower + internal |
| move to external underwriting | R-IA (on provider+borrower confirmation) | EOI accepted by borrower | — | opportunity.external_underwriting | back to published | all parties (status) |
| pause | R-IA/R-BA request | reason | — | opportunity.paused | resume | providers see "paused" |
| funded externally | R-IA (borrower/provider report) | outcome details (taxonomy 37) | — | opportunity.funded_external | correction w/ reason | all parties |
| declined externally | R-IA | decline reason taxonomy | — | opportunity.declined_external | — | borrower + internal |
| withdraw | R-BA | — | — | opportunity.withdrawn | new opportunity later | providers lose access (grace: none) |
| close / archive | R-IA | terminal outcome recorded | — | opportunity.closed/archived | — | read-only history |

## 6. Cross-cutting rules

1. **Access follows state**: provider access is valid only in `published`…`in_external_underwriting`, unexpired grant, unexpired assessment.
2. **Expired assessment poisons downstream**: assessment.expired forces opportunity `paused` with reason "stale readiness" until reassessment.
3. **Suspension cascade**: borrower org suspension pauses opportunity and assessment; provider org suspension revokes grants.
4. **No skipping**: transitions not listed are invalid; engine rejects and logs `workflow.invalid_transition_attempted`.
