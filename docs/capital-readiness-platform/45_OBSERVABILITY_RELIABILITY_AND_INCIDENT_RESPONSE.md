# 45 — Observability, Reliability, and Incident Response

| Field | Value |
|---|---|
| Purpose | Monitoring, SLOs, degradation behavior, incident management (including AI incidents), and DR |
| Audience | Engineers, ops |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Backend lead |
| Dependencies | 16 (NFRs), 30 (pipelines), 44 (security), 46 (environments) |
| Source references | CRF §16 layer 11–12 |
| Assumptions | ASM-05 |
| Open questions | — |
| Approval required | Engineering lead |
| Last updated | 2026-07-16 |

## 1. Telemetry

Structured JSON logs with correlation IDs (request → workers → events); metrics (RED per endpoint, queue depths, pipeline stage timings, scoring recompute time, cost counters); distributed traces on API + pipeline paths; **no P3/P4 in telemetry** (SB-14). Retention 13 months (RC-TELEM).

## 2. SLOs & alerts

| SLO | Target (16) | Page/alert |
|---|---|---|
| Availability (borrower/provider surfaces) | 99.5% monthly | page on burn rate |
| API latency | P95 < 500ms read | alert |
| Doc feedback pipeline | ≤10 min P95 (NFR-04) | alert at 15 min queue age; degrade at 30 |
| Scoring recompute | <5s | alert |
| Audit chain verification | daily pass | page on failure (integrity.hash_mismatch = Sev-1) |
| Notification delivery | <1% failure | alert |
| AI cost budget | monthly cap (NFR-20) | alert 80%, breaker 100% |

## 3. Dashboards

Ops (queues, SLAs, errors), pipeline health (30), security (44 SIEM views), cost, and methodology-health (37) — each SLO above has a chart + alert (NFR-13).

## 4. Graceful degradation matrix

| Failure | Behavior |
|---|---|
| LLM/doc-AI vendor down | pipelines pause → documents flow to manual classification/entry queues; borrower messaging "under review" (no error); ops alert |
| Queue backlog | autoscale workers; prioritize per-document over batch lanes |
| Email provider down | in-app remains; queued resend; critical notices retried first |
| Object storage degraded | uploads rejected with retry guidance; reads via cache where possible |
| DB failover | managed failover; app retries idempotently (NFR-19) |
| Audit store unavailable | **blocking**: state-changing actions fail closed (36 §4) — page immediately |

## 5. Incident response

Severities: Sev-1 (data breach, cross-tenant leak, audit integrity, score-integrity defect, GATE bypass), Sev-2 (surface outage, pipeline halt >4h, vendor breach notice), Sev-3 (degradations), Sev-4 (minor). Flow: detect → incident commander → comms plan (status page internal; borrower/provider notice per severity; regulator/data-subject notification per breach law — counsel-guided, 64) → contain/eradicate → post-incident review ≤5 business days with action items tracked. Tabletop exercises: security breach + AI-incident scenarios pre-launch (SB-18).

**AI incidents** (31 §8): hallucinated value reaching a surface, citation failure, P4 leak into prompt/output, injection success, systematic extraction error → treated as Sev-2 minimum, AI bundle rollback runbook (revert to previous bundle version; re-queue affected docs; notify framework owner if any assessment consumed bad data → reassessment_required).

**Score-integrity incidents:** any defect where published ≠ correct computation → Sev-1: freeze affected assessments (suspend state), recompute under pinned versions, notify affected borrowers/providers with correction (57 correction process), document in decision log.

## 6. DR

RTO 4h / RPO 1h (NFR-05); cross-region backups (35 §6); quarterly restore drill + annual full DR exercise; runbooks versioned in repo.
