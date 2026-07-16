# 17 — UX Information Architecture

| Field | Value |
|---|---|
| Purpose | Navigation structure, surface map, and IA principles for the four experiences (public, borrower, internal, provider) |
| Audience | Product design, frontend engineering |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Design lead |
| Dependencies | 18 (screen specs), 13 (journeys), 12 (permissions) |
| Source references | D-03, D-06, D-08 |
| Assumptions | ASM-03 (ES-first borrower UI) |
| Open questions | — |
| Approval required | Design + product |
| Last updated | 2026-07-16 |

## 1. IA principles

1. **One workspace, one mental model:** borrower IA mirrors the readiness methodology — Profile → Evidence → Results → Actions — so the product teaches the framework.
2. **Status is always one glance away:** every surface has a persistent status header (lifecycle state + next expected action + SLA hint).
3. **Explanations co-located with numbers:** no score or grade renders without its "what this means / what it is not" affordance (02 §3).
4. **Queues, not inboxes, for internal users:** work is pulled from typed queues with SLAs, never from unstructured lists.
5. **Provider surface is read-heavy and comparison-oriented:** minimal chrome, evidence-first, everything exportable-with-watermark.
6. **Progressive disclosure:** dimension → control → evidence drill-down everywhere results appear.

## 2. Surface map

### Public (unauthenticated, ES/EN)
```
/                       Landing (SCR-P1)
/apply                  Business application (SCR-P2)
/providers/apply        Capital-provider application (SCR-P3)
/partners/apply         Partner application (SCR-P4)
/status                 Application status (magic link) (SCR-P8)
/legal/*                Terms, privacy, fee disclosure (FR-ADM-03)
```

### Borrower workspace (`app.` — role R-BA/R-BM)
```
Dashboard (SCR-B1)
├── Company
│   ├── Profile (SCR-B2)
│   ├── Ownership & management (SCR-B4)
│   └── Report a change (SCR-B3)
├── Capital request (SCR-B5)
├── Evidence
│   ├── Checklist (SCR-B6)
│   ├── Document room (SCR-B7)  [upload flow + doc-analysis results]
│   └── Clarifications (SCR-B8)
├── Tasks (SCR-B9)
├── Results
│   ├── Readiness score (SCR-B10)
│   ├── Dimension details (SCR-B11)
│   ├── Flags & conditions (SCR-B12)
│   ├── Action plan (SCR-B13)
│   ├── History (SCR-B14)
│   └── Readiness report (SCR-B15)
├── Capital-provider activity (SCR-B16)
└── Settings (org members, notifications, consents)
```

### Internal administration (`ops.` — internal roles per queue permission)
```
Home: my queues + SLA summary (SCR-A0)
├── Applications: queue (SCR-A1) → review (SCR-A2)
├── Organizations (SCR-A3)
├── Assessments: queue (SCR-A4)
│   ├── Document review (SCR-A5)
│   ├── AI exceptions (SCR-A6)
│   ├── Control assessment (SCR-A7)
│   ├── Gates & flags (SCR-A8)
│   └── Override approvals (SCR-A9)
├── Opportunities: packaging (SCR-A10) → provider access (SCR-A11)
├── Audit log (SCR-A12)
└── Configuration
    ├── Scoring model (SCR-A13)
    └── Jurisdictions (SCR-A14)
```

### Capital provider (`capital.` — R-CPA/R-CPN)
```
Dashboard (SCR-C1)
├── Opportunities: list (SCR-C2) → profile (SCR-C3)
│   ├── Readiness summary (SCR-C4 tab)
│   ├── Document room (SCR-C4b tab)
│   ├── RFIs (SCR-C5 tab)
│   ├── EOI (SCR-C6)
│   └── Team notes (SCR-C7 tab)
├── Saved opportunities (SCR-C8)
└── Status tracking (SCR-C9)
```

## 3. Global components

Status header; limitation-statement component (single shared implementation); evidence citation chip (links any number/claim to its source doc+page); confidence badge (A–D with tooltip); band chip (5 bands, colorblind-safe palette); flag chip (severity-coded); AI-assist label ("AI-assisted draft — verified by <name>" or "pending verification"); audit-context footer on internal detail views (who did what last).

## 4. Navigation & state rules

- Deep links stable per object ID; queue items open in context-preserving panels.
- Locale switch persists per user; borrower default ES, internal default EN (ASM-03).
- Unsaved-changes guards on all multi-step forms; autosave on application and profile forms.
- Empty/loading/error state patterns standardized in 18 §1 and reused.
