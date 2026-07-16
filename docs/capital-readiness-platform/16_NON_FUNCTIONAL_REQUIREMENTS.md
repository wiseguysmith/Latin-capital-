# 16 — Non-Functional Requirements

| Field | Value |
|---|---|
| Purpose | Testable quality attributes: performance, availability, security, privacy, scalability, operability, accessibility, localization |
| Audience | Engineering, security, QA, ops |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Backend lead + Security lead |
| Dependencies | 40, 44, 45, 73 |
| Source references | Master prompt §8, §13; CRF §16–18 |
| Assumptions | ASM-05 (cloud), ASM-11 (file limits); pilot volumes per §1 |
| Open questions | OQ-04 (residency) |
| Approval required | Engineering + security leads |
| Last updated | 2026-07-16 |

## 1. Sizing assumptions (pilot horizon, 12 months)

≤500 applicant records; ≤100 active workspaces; ≤50 concurrent users; ≤20k documents (~200 GB); ≤20 provider orgs. NFRs below are set for 10× these numbers to avoid re-architecture (D-01 expandability).

## 2. Requirements

| ID | Category | Requirement | Measure / test |
|---|---|---|---|
| NFR-01 | Performance | Interactive page loads P95 < 2.5s, API reads P95 < 500ms at 10× pilot load | Load test TC-NFR-01 |
| NFR-02 | Performance | Document upload 100MB completes over 10Mbps without timeout; resumable chunks | TC-NFR-02 |
| NFR-03 | Performance | Deterministic scoring recompute < 5s per assessment | TC-NFR-03 |
| NFR-04 | Performance | Per-document AI feedback available ≤10 min P95 after upload (queue+pipeline) | Pipeline SLO dashboard |
| NFR-05 | Availability | 99.5% monthly availability for borrower/provider surfaces (pilot); RTO 4h, RPO 1h | DR test 45 §6 |
| NFR-06 | Durability | Documents and audit log: 11-nines object durability + cross-region backup; no single-region loss destroys evidence | Backup restore drill quarterly |
| NFR-07 | Security | All NFR-security controls of 44 §2 baseline implemented before any real borrower data (TLS1.2+, AES-256 at rest, MFA, RBAC deny-default, signed URLs ≤15 min TTL, malware scanning, secrets vaulted) | Security review gate 77 |
| NFR-08 | Security | Audit log append-only with tamper-evidence (hash chain verified daily) | TC-AUD-02 |
| NFR-09 | Security/Privacy | Access revocation (user, org, consent, grant) effective ≤1h; session revocation ≤5 min | TC-SEC-05 |
| NFR-10 | Privacy | PII (P4) encrypted with distinct keys; field-level access logged; no P4 in logs/analytics (DLP scan) | TC-SEC-07 |
| NFR-11 | Privacy | Data-subject export/erasure requests executable ≤30 days respecting legal holds (35 §5) | Ops runbook drill |
| NFR-12 | Scalability | Stateless app tier horizontally scalable; queue-based AI pipeline scales workers independently; DB read replicas supported | Architecture review |
| NFR-13 | Operability | Structured logs w/ correlation IDs; metrics + traces per 45; alert coverage for every SLO here | 45 §3 checklist |
| NFR-14 | Operability | All configuration (overlays, scoring models, thresholds) hot-reloadable without deploy; versioned | TC-CONF-01 |
| NFR-15 | Auditability | Any assessment result reproducible from stored inputs + pinned model version, ≥7 years | TC-SCORE-02 |
| NFR-16 | Accessibility | WCAG 2.1 AA for borrower and provider surfaces | Axe audit in CI |
| NFR-17 | Localization | Full ES/EN string coverage; no concatenated translatable strings; legal texts versioned per locale + jurisdiction | i18n lint |
| NFR-18 | Compatibility | Latest 2 versions of Chrome/Edge/Safari/Firefox; responsive ≥360px width (no native apps, 10 §3) | BrowserStack matrix |
| NFR-19 | Data integrity | All state transitions ACID; document hash verified on read; idempotent external-facing mutations | TC-INT-01 |
| NFR-20 | AI cost/latency | Per-assessment AI cost budget tracked; hard monthly cap with graceful degradation to manual queues (31 §10) | Cost dashboard |
| NFR-21 | Retention | Retention/deletion jobs per 35 schedule with destruction certificates | TC-RET-01 |
| NFR-22 | Multi-tenancy | Org-scoping enforced at data layer (RLS or equivalent) — cross-tenant read is a Sev-1 defect class | Pen test + TC-SEC-01 |

## 3. Explicit non-goals (pilot)

99.9%+ availability, active-active multi-region, sub-100ms APIs, offline mode, real-time collaboration editing. Revisit at Phase 4.
