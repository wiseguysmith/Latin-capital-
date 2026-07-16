# 71 — Prioritized Product Backlog

| Field | Value |
|---|---|
| Purpose | Ordered delivery backlog with critical path, manual-first designations, and dependency notes |
| Audience | Engineering leads, product, program management |
| Status | Draft v1.0 — reorder at sprint planning, never silently reprioritize P0 safety items |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | 70 (stories), 75 (phases) |
| Source references | Master prompt §17 (critical path, manual-first) |
| Assumptions | ASM-02; 2-week sprints |
| Open questions | — |
| Approval required | Product owner |
| Last updated | 2026-07-16 |

## 1. Critical path (must land in order)

```
US-101/102 (envs+security baseline)
 → US-105/104 (auth+orgs)
 → US-103 (audit spine)                       ← everything depends on it
 → US-201/202/204 (intake→provisioning)
 → US-301–304 (workspace+checklist)
 → US-401–403 (documents manual review)
 → US-601/609 (scoring engine + model config)
 → US-602–606, 608, 610 (assessment workflow)
 → US-611 (borrower results/report)           ← Phase 1 complete
 → US-501–508 (AI pipelines)                  ← Phase 2
 → US-801–808 (provider workspace)            ← Phase 3 (gated by US-1301 counsel memo)
```

Parallelizable off-path: US-901 (notifications, needed by Phase-1 end), US-1001/1002 (with audit spine), US-1101 (overlay, needed before US-304), EP-12 analytics (from Phase-2), EP-13 legal (starts day 1 — longest external lead time).

## 2. Sprint-order backlog (pilot build; ~2-week sprints, adjust to team)

| Sprint | Contents | Milestone |
|---|---|---|
| S1 | US-101, 102 (partial), 105 | envs live |
| S2 | US-102 (done), 103, 104 | audit spine + org admin |
| S3 | US-201, 202, 203 | intake E2E on staging |
| S4 | US-204, 301, 302, 1101 | workspace + CR overlay v1 |
| S5 | US-303, 304, 401 | checklist + uploads |
| S6 | US-402, 403, 701, 704 | manual doc review operable |
| S7 | US-601, 609 | scoring engine + golden vectors green |
| S8 | US-602, 603, 605, 608 | control assessment + gates/caps |
| S9 | US-604, 606, 610 | flags, overrides, final approval |
| S10 | US-611, 901, 1005 | **Phase 1 exit: first in-product assessment** |
| S11–S14 | EP-05 (501→508), 702, 703, 607, 612 | **Phase 2 exit: AI-assisted assessment verified** |
| S15–S18 | EP-08 (801→808), 1003, 705, 613, 614, 1002 | **Phase 3 exit: first provider review** |
| cont. | EP-12, US-902, 1004, 205, 305/306 | hardening + pilot ops |

## 3. Manual-first designations (do NOT build early; master prompt §17)

| Process | Stays manual until | Rationale |
|---|---|---|
| Applicant screening protocol (sanctions/adverse media) | Phase 4 vendor (OQ-07) | list coverage unproven; volume low |
| Identity verification | Phase 2+ vendor | ASM-06 |
| Billing/invoicing | post-pilot (OQ-01) | model unfixed (D-15) |
| Provider onboarding & curation | indefinitely manual in MVP | D-18; judgment-heavy |
| Evidence-confidence grading | Phase 2 (compute), manual entry Phase 1 | calibration first (OQ-15) |
| External Risk Context | manual library selection | expert judgment |
| Registry checks (CR) | Phase 4 integration | manual lookup protocol works |
| Report layout QA | each release | correctness > automation |

## 4. Re-prioritization rules

P0 safety/security/audit items cannot be traded for features; scope changes touching 02 boundaries need founder+counsel (10 §4); anything blocking the Phase-gate exit criteria (75) outranks in-phase polish; new items enter via D-entry or FR addition, then 79 traceability update.
