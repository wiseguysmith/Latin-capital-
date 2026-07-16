# 36 — Audit Log and Event Model

| Field | Value |
|---|---|
| Purpose | Canonical audit/event architecture: event schema, naming registry, immutability, and query/report obligations |
| Audience | Backend engineers, compliance, auditors |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Backend lead + Compliance |
| Dependencies | 14 (transitions), 33 (ENT-30), 44 (integrity) |
| Source references | CRF §16 layer 12; §18 (auditability); D-07 (all material human actions recorded) |
| Assumptions | Hash-chain tamper evidence (ASM: standard practice) |
| Open questions | — |
| Approval required | Compliance |
| Last updated | 2026-07-16 |

## 1. Event schema (ENT-30)

```
AuditEvent {
  event_id (uuidv7), event_name (dot.namespaced),
  occurred_at (UTC), recorded_at,
  actor {user_id | service_id, org_id, role, on_behalf_of?, break_glass?: bool},
  object {type, id, workspace/opportunity scope},
  action_context {prior_state?, new_state?, reason_code?, evidence_refs?, request_id, ip?, user_agent?},
  payload (event-specific, P4-redacted),
  model_versions? {scoring, overlay, ai_bundle},
  chain_hash  // sha256(canonical(event) ‖ previous chain_hash), per-tenant chain
}
```

Rules: append-only store; no UPDATE/DELETE grants to any application role; corrections are new events (`*.corrected` referencing the original); daily chain verification job (NFR-08); clock from trusted source; events written in the same transaction as the state change (transactional outbox → audit store).

## 2. Event registry (canonical names)

### Lifecycle events (already bound in 14)
`applicant.draft_created|submitted|review_started|info_requested|accepted|waitlisted|rejected|withdrawn`
`assessment.workspace_activated|collection_started|package_analysis_started|review_required|clarification_requested|remediation_started|ready_for_final|final_review_started|approved|approved_with_conditions|marked_not_ready|expired|reassessment_required|suspended`
`document.requested|uploaded|processing_started|classified|extraction_completed|verification_required|verified|rejected|superseded|expired|deleted_under_policy|legal_hold_applied|legal_hold_released|quarantined`
`opportunity.created|packaged|publication_requested|publication_approved|published|rfi_submitted|rfi_answered|eoi_submitted|eoi_responded|external_underwriting|paused|resumed|funded_external|declined_external|withdrawn|closed|archived`

### Human-decision events (D-07 mandatory)
`screening.completed|dispositioned` · `gate.dispositioned` · `flag.created|updated|resolved|restricted_applied|restricted_reviewed` · `control.assessed|section_signed` · `override.proposed|approved|rejected|expired` · `exception.created|resolved` · `attestation.signed` · `consent.granted|revoked` · `change.reported`

### Access & security
`auth.login|logout|mfa_enrolled|mfa_failed|password_reset` · `access.granted|extended|revoked|expired` · `document.viewed|downloaded` (all roles; provider events carry watermark ref) · `package.downloaded` · `report.viewed` · `audit.exported` (dual-approved) · `breakglass.opened|closed` · `org.created|suspended|reinstated` · `membership.invited|accepted|removed|role_changed`

### Configuration & models
`scoring_model.drafted|validated|published|retired` · `overlay.drafted|published` · `ai_bundle.registered|shadow_started|activated|rolled_back` · `threshold.changed` · `template.published`

### System & data
`workflow.invalid_transition_attempted` · `pipeline.failed|retried|degraded` · `retention.executed` (destruction certificate) · `notification.sent|failed` · `integrity.hash_mismatch` (Sev-1) · `dlp.violation_detected`

## 3. Query & reporting obligations

- Viewer (SCR-A12): filter by actor/object/event/time; object timelines assembled per entity ("show me everything about assessment X").
- Standing reports: override register (23 §7), restricted-flag review (23 §6), break-glass review (12 §7), provider access/download report per opportunity (ASM-12), consent ledger extract, screening disposition log.
- Examiner package (CRF §16 layer 12): reproducible export for a given workspace/period with chain verification proof — dual-approval to export (FR-AUD-01).

## 4. Non-audit telemetry separation

Product analytics/ops telemetry (37, 45) are separate streams; audit events are never sampled, never dropped on backpressure (backpressure blocks the action instead), and contain no P4 values in payloads (references only).
