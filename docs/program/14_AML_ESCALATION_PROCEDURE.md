# 14 — AML/CFT Escalation Procedure (SOP)

| Field | Value |
|---|---|
| Purpose | Operationalize capitalYA/CRP's **complementary** AML/CFT controls and the escalation of suspicious activity to the partner lender's MLRO — without capitalYA making regulated determinations or tipping off |
| Audience | capitalYA/CRP compliance & ops, partner lender MLRO, counsel |
| Status | **DRAFT SOP — requires counsel confirmation of registration/reporting obligations** (`03_...` §17 Q11–Q12) |
| Version | 1.0.0 |
| Owner | Compliance lead |
| Dependencies | `02_...` §7, `03_...` §8, `05_...` §3.8, `06_...` §6, `07_...` §4.3; CRP `36`, `44` |
| Last updated | 2026-07-19 |

---

## 1. Roles

- **Partner lender** = the **primary regulated AML reporting entity** for loan origination and financial transactions; owns SAR/suspicious-transaction filing, investigation of record, and regulator responses.
- **capitalYA/CRP** = **complementary, risk-based** controls, because they collect identity/ownership data, introduce borrowers, screen documents, may spot suspicious conduct first, may serve foreign investors (Horizon 2), and may facilitate digital-asset transactions later (`03_...` §8.1). capitalYA files independently **only** if its own registration status requires it (counsel-determined).

## 2. Controls capitalYA/CRP perform (risk-based)

Identity verification (individual & entity); UBO identification; PEP screening; **sanctions screening**; adverse-media screening; source-of-funds review; source-of-wealth review (higher-risk); geographic-risk classification; transaction monitoring (pre-handoff signals); wallet analytics **before** any tokenization (Horizon 2); record retention; periodic customer refresh (`03_...` §8.1).

Results are recorded as **preliminary / non-relied** (`07` §4.3); the lender re-performs KYC of record.

## 3. Sanctions lists (minimum)

UN lists; Costa Rican and applicable local lists; **OFAC** where US persons/infrastructure/USD clearing/US counterparties are involved; any list required by the partner bank/lender (`03_...` §8.1).

> **Accreditation ≠ reduced diligence.** No investor receives simplified KYC merely for being wealthy/accredited; accreditation addresses eligibility, not identity/sanctions risk (`03_...` §8.2).

## 4. Escalation flow (no tipping off)

```
Signal detected (screening hit / anomaly / document red flag / behavior)
   → Analyst logs it (audit event, CRP 36) — factual, no accusation to the customer
   → Compliance triage (severity, false-positive check)
   → Escalate to PARTNER LENDER MLRO via the secure channel
        • transmit observation + evidence
        • DO NOT inform or hint to the customer (no tipping off)
   → Lender MLRO investigates and decides on filing (of record)
   → capitalYA records the escalation + outcome reference; retains case file
   → capitalYA files independently ONLY if its own registration requires (counsel)
```

### 4.1 Sanctions — hard rule

capitalYA **never auto-clears a sanctions match** (`05` §3.8; CRP `02` §2.7). Any potential match sets `sanctions_potential_match_escalated: true` (`07` §4.3) and routes to the lender MLRO for determination. capitalYA may pause the workflow but does not adjudicate.

## 5. Responsibility matrix (fixed in the partner agreement)

| Item | Owner |
|---|---|
| Who files regulatory/suspicious reports | **Lender** (capitalYA only if own registration requires) |
| Who investigates alerts | Lender MLRO (capitalYA triages) |
| Who retains the case file | Both (each its own records); lender of record |
| Who responds to regulator information requests | Lender (capitalYA cooperates) |
| How capitalYA transmits observations | Secure channel, no tipping off |

The lender **cannot** "own all AML" while capitalYA operates blind; capitalYA cannot assume the lender's reporting role (`03_...` §8.1).

## 6. Records & training

- Retain screening results, escalations, and outcomes per schedule (aligned to CRP `35`; AML retention of record with lender).
- All escalations are **audit events** (CRP `36`).
- Staff trained on tipping-off prohibition, sanctions escalation, and this SOP.

## 7. Secondary markets (Horizon 2 — deferred)

El Salvador (CNAD issuers/PSADs), Colombia (SFC + UIAF), Brazil (CVM/BCB) partners perform regulated AML of record; capitalYA maintains a **complementary group-level program** (`03_...` §8.2).

## 8. Open items → counsel

`[Which entity registers under AML Art. 15 / 15 bis]`, `[capitalYA independent-filing triggers]`, `[exact thresholds by entity/transaction/currency — the "₡10,000" figure is NOT used]` (`03_...` §8.1), `[MLRO contact + channel spec]`.
