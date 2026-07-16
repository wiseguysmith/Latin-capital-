# 41 — Service and Module Boundaries

| Field | Value |
|---|---|
| Purpose | Module decomposition, ownership, dependency rules, and interface contracts inside the modular monolith |
| Audience | Engineers |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Backend lead |
| Dependencies | 40 (architecture), 42 (APIs), 33 (entities) |
| Source references | CRF §16; P3 (deterministic/AI separation) |
| Assumptions | ADR-001/002 |
| Open questions | — |
| Approval required | Backend lead |
| Last updated | 2026-07-16 |

## 1. Module registry

| Module | Responsibility | Owns entities (33) | May call | Must never call |
|---|---|---|---|---|
| `identity-access` | orgs, users, memberships, invitations, grants, sessions, segregation checks | ENT-01…04 | notifier | domain modules |
| `intake` | applicants, screening, decisions | ENT-01(applicant view), 02 | identity-access, notifier, workflow | scoring, ai |
| `workspace` | profiles, ownership graph, capital requests, checklists, tasks | ENT-05…10 | rules-gates (checklist gen), workflow, notifier | scoring internals |
| `evidence` | documents, versions, storage, extractions, fields, links, exceptions | ENT-11…15 | storage, ai-orchestrator (enqueue only), workflow | scoring internals |
| `rules-gates` | applicability profiles, checklist composition, gates, freshness, caps triggers | config from ENT-31/32 | — (pure rules over inputs) | ai |
| `scoring-engine` (lib) | 20 §4 computation, result objects | ENT-17…22 (compute) | **nothing external** (pure) | network, ai, clock |
| `assessment` | assessment lifecycle, control assessments, flags, overrides, results persistence, reports orchestration | ENT-16…23, 36(reports) | scoring-engine, rules-gates, ai-orchestrator (drafts), workflow, notifier | — |
| `opportunity` | packaging, publication, grants, RFI/EOI, outcomes | ENT-25…29 | identity-access (grants), assessment (read), notifier, workflow | scoring mutation |
| `ai-orchestrator` | pipelines, model bundles, thresholds, queues to humans (30) | ENT-12 (writes), AI registry | vendors, evidence (write extractions) | workflow transitions, scoring, permissions |
| `workflow` | state machines (14), transition validation, transactional outbox | transitions | audit | ai |
| `audit` | append-only events, chains, exports (36) | ENT-30 | — | — |
| `notifier` | templates, channels, digests (19) | ENT-33 | email provider | domain writes |
| `config-registry` | scoring models, overlays, AI bundles, thresholds; draft/publish workflow | ENT-31/32 | audit | — |
| `admin-reporting` | queues views, dashboards feeds (37) | read-only projections | all (read) | writes |

## 2. Dependency rules (CI-enforced)

1. `scoring-engine` imports nothing but stdlib + decimal; consumed as versioned package (ADR-002). Test TC-AI-01 asserts no AI client in its dependency graph.
2. `ai-orchestrator` outputs land only in `evidence` (extractions/fields marked unverified) or human queues — it has no write access to workflow states, scores, permissions, or audit-except-its-own events (31 §2).
3. All state changes route through `workflow`; modules request transitions, workflow validates (14 §6 no-skipping) and emits audit atomically.
4. Cross-module access via exported interfaces only (no shared table access); read-model projections for reporting.
5. `identity-access` authz middleware wraps every API route; object-level checks in modules (12 §1 defense in depth).

## 3. Interface contracts (representative)

```
rules-gates.generateChecklist(profileId, overlayVersion, request) → ChecklistItem[]
rules-gates.evaluateGates(assessmentId) → GateStatus[]
scoring-engine.compute(AssessmentInput) → AssessmentResult          // pure
assessment.requestTransition(assessmentId, transition, actorCtx)    // → workflow
ai-orchestrator.enqueueDocument(docId, bundleVersion)
ai-orchestrator.enqueuePackageAnalysis(assessmentId)
opportunity.package(assessmentId, disclosureSet) → lint report | artifacts
audit.append(event) // called only via workflow/outbox helpers
```

## 4. Extraction path (post-MVP)

First candidates for service extraction under load: `ai-orchestrator` workers (already async), `notifier`, document preview/render. `scoring-engine` stays a library everywhere (including future services) to preserve purity and reproducibility.
