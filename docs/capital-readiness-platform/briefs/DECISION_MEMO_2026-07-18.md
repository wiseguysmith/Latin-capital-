# Decision Memo — Pilot Business Decisions

**Date:** 2026-07-18 · **Decided by:** Founder · **Recorded in:** `05_DECISION_LOG.md` (D-21, D-22, D-23)
**Status of documentation suite:** complete (69 documents, pushed to `claude/latin-capital-repo-iur2ew`); formal review/approval of the suite remains open (D-14 gate)

---

## Decision 1 — How the pilot makes money (D-21; closes OQ-01)

**Decision:** Businesses pay a **single flat, disclosed assessment fee** (indicative USD $2,500–$5,000; founder may discount to zero for the first pilot cohort). No success fees, no percentage-of-funding fees, no subscription, and **no fees from capital providers** during the pilot. Invoicing stays manual (outside the product).

**Why:** Payment for work performed, not for funding outcomes — the cleanest position against being characterized as a broker or intermediary, and the easiest way to defend the principle that *a score can never be bought*. Counsel will still confirm the fee's legal characterization (brief item CR-L1).

**What it changes:** fee-disclosure page shows one fee; pilot agreements state it plainly; the scoring "fee firewall" (payment status can never touch a score) is unchanged and remains test-enforced.

## Decision 2 — Company structure (D-22; closes OQ-02, pending counsel confirmation)

**Decision:** **One operating company** runs the Costa Rica pilot. No Costa Rican subsidiary unless counsel advises it is required (brief items CR-L1/CR-L2).

**Why:** Simplest and cheapest structure while the business model is unproven; local incorporation or a holding structure can be added later without unwinding anything.

**What it changes:** all pilot contracts (borrower terms, provider agreements, partner agreements) issue from the single entity; the counsel brief asks explicitly whether that entity has any Costa Rican registration duty.

## Decision 3 — Where sensitive data lives (D-23; closes OQ-04, pending counsel confirmation)

**Decision:** Borrower documents and beneficial-owner identity data are stored **encrypted in a major US cloud region**, with each business giving **explicit written consent** to the cross-border transfer, and AI vendors contractually barred from training on or retaining the data. The architecture keeps the storage region swappable, so if counsel's privacy analysis (CR-L3) requires relocation, it is a configuration change, not a redesign.

**Why:** Mature tooling, lowest cost, fastest build — and Costa Rica's privacy law is consent-centric, making this the likely-confirmable default rather than a gamble.

**What it changes:** infrastructure setup can begin once the suite is approved; consent texts (Spanish, counsel-validated) become a launch-blocking dependency, already flagged in the launch checklist.

## Immediately actioned

1. Decision log and open-questions register updated (this memo is the narrative record).
2. **Counsel brief prepared** (`briefs/COUNSEL_BRIEF_COSTA_RICA.md`) — ready to send to a Costa Rican firm today; legal review is the longest-lead item on the plan.

## Still open (the last gate)

- **Formal review and approval of the documentation suite by the founder (D-14).** Engineering build does not start until this is given.
