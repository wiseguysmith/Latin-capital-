# 40 — System Architecture

| Field | Value |
|---|---|
| Purpose | Overall technical architecture: topology, stack posture, CRF-layer mapping, and key architectural decisions |
| Audience | All engineers |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Backend lead / architect |
| Dependencies | 41 (boundaries), 42 (APIs), 43–46 |
| Source references | CRF §16 (12-layer reference architecture); NFRs (16) |
| Assumptions | ASM-05 (cloud), ASM-15 (AI vendors); stack choices are recommendations pending team confirmation (ADR-001…) |
| Open questions | OQ-04 (region) |
| Approval required | Engineering lead |
| Last updated | 2026-07-16 |

## 1. Architectural style

**Modular monolith + separated workers** for MVP: one deployable application with strictly enforced internal module boundaries (41), plus asynchronous worker fleet for AI pipelines and jobs, plus isolated deterministic **scoring engine** as an internally-versioned library invoked by the app (no AI dependencies — 31 §2). Rationale: pilot scale (16 §1), small team (ASM-02), highest-risk complexity is domain logic not distribution. Extraction to services post-MVP only where scaling demands (80).

```
[Web clients: borrower / internal / provider SPAs (one app, role-gated)]
        │ HTTPS
[API gateway / BFF]──[AuthN/Z service (43)]
        │
[Core application (modular monolith, 41)]
  ├── intake · workspace · evidence · assessment · opportunity · admin modules
  ├── scoring-engine lib (deterministic, versioned)   ← no AI imports (CI-enforced)
  ├── rules & gates engine (checklists, applicability, freshness)
  └── workflow engine (14 state machines)
        │ transactional outbox
[Event bus / queues] ──► [Workers: doc-intel pipelines (30), notifications,
                          retention jobs, staleness sweeps, exports]
        │
[PostgreSQL (RLS multi-tenant)] [Object storage (35)] [Audit store (36, append-only)]
        │
[External: LLM API · Document-AI/OCR · e-sign · KYC vendor (phase) · email]
```

## 2. CRF §16 layer mapping

| CRF layer | Implementation |
|---|---|
| 1 Secure ingestion | upload service + malware scan + hash + consent check (35, FR-DOC-01) |
| 2 Document intelligence | doc-intel workers (30) |
| 3 Financial normalization | normalizer worker + deterministic metrics service (32 §3, 34 §3) |
| 4 Entity/ownership graph | relational graph (ENT-06/07) + validators |
| 5 Verification layer | verification queues + partner evidence (52) — vendor integrations Phase 4 |
| 6 Rules & gates engine | deterministic module (checklists 24/26, gates 23, freshness) |
| 7 Scoring engine | versioned deterministic lib (20) |
| 8 LLM reasoning | drafting workers, human-gated (31) |
| 9 Matching engine | **not in MVP** (D-18); mandate data captured (ENT-28) |
| 10 Workflow & human review | workflow engine + queues (14, 50) |
| 11 Monitoring/early warning | staleness/expiry jobs MVP; portfolio monitoring post-MVP (80) |
| 12 Audit & model governance | audit store (36) + model registry (31 §7) |

## 3. Stack recommendations (ADRs to confirm at build kickoff)

| Layer | Recommendation | Rationale |
|---|---|---|
| Backend | TypeScript (NestJS-class) or Python (FastAPI-class) monolith; **decimal library mandatory for scoring** (D-20) | team-dependent; both have mature ecosystems |
| DB | PostgreSQL 16+ with row-level security for tenant scoping (NFR-22) | RLS + JSONB for config bundles |
| Frontend | React SPA(s) with typed API client; single design system (17 §3) | shared components incl. limitation-statement |
| Queue | Cloud-native queue (SQS-class) + outbox pattern | at-least-once with idempotent consumers (NFR-19) |
| Object storage | S3-class with object lock (35) | WORM originals |
| AI | hosted LLM API + document-AI vendor (ASM-15; selection 47) | no self-hosted models in MVP |
| Infra | IaC (Terraform), containers on managed runtime (ECS/Cloud Run-class) | 46 |
| AuthN | managed IdP (Auth0/Cognito-class) with MFA | 43 |

## 4. Key architectural decisions (ADR summaries)

- **ADR-001 Modular monolith** (above). Revisit trigger: >3 teams or >10× pilot load.
- **ADR-002 Scoring engine as pure library** with its own semver + model-bundle config; embedded in app and in a CLI for offline recompute/audit (NFR-15).
- **ADR-003 Configuration as versioned data**: scoring models (ENT-31), overlays (ENT-32), AI bundles, thresholds — all runtime-loadable, pinned per assessment (NFR-14). No business constants in code.
- **ADR-004 Single relational source of truth**; audit store is the only additional write path (transactionally coupled via outbox).
- **ADR-005 One web app, three shells** (borrower/internal/provider) sharing the design system but with separate route trees and bundles — prevents cross-surface component leakage of sensitive widgets (12).
- **ADR-006 Region:** single cloud region acceptable for pilot pending OQ-04; architecture keeps residency swappable (no region-hardcoded services; storage/DB endpoints via config).

## 5. Failure & scale posture

Stateless app tier (NFR-12); queue-buffered AI with degradation to manual (30 §7); DB vertical headroom + read replicas; graceful-degradation matrix in 45 §4. Capacity target: 10× pilot (16 §1) without redesign.
