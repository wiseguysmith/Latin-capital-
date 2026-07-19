# 16 — Lender-Failure & Backup-Servicing Plan

| Field | Value |
|---|---|
| Purpose | Define what happens to outstanding loans and borrowers if the partner lender loses its authorization or fails — so the program winds down safely and capitalYA never assumes regulated lending functions |
| Audience | Founder, counsel, compliance, ops, partner lender, backup servicer |
| Status | **DRAFT PLAN — binding intent to be codified in the partner agreement (`10`/`04` §5)** |
| Version | 1.0.0 |
| Owner | Founder + Compliance + counsel |
| Dependencies | `03_...` §4.3, `04_...` §5, `05_...` §3.12, `06_...` §8, `10_...` §10 |
| Last updated | 2026-07-19 |

---

## 1. Guiding principle

Outstanding loans **do not disappear** if the lender fails, and capitalYA **does not inherit** the lender's regulated role. A partner failure is a **wind-down event, not a promotion** (`03_...` §4.3; `05` §3.12). capitalYA continues only its technology/servicing-support role and coordinates an orderly transition.

## 2. Triggers

- Loss, suspension, or non-renewal of the lender's authorization.
- Insolvency, receivership, or regulatory intervention.
- Material breach of the partner agreement affecting its ability to originate/service.
- `[Other triggers per agreement]`.

Any trigger activates this plan automatically under the partner agreement.

## 3. Immediate actions (auto-triggered)

1. **Stop new originations.**
2. **Stop any token issuance** (Horizon 2 safeguard).
3. **Move collections to an approved account** (lender's successor / trustee — never a capitalYA account).
4. **Appoint the backup servicer** (identified in advance).
5. **Require delivery of complete borrower and loan files** from the lender.
6. **Transfer or assign loans** where legally permitted.
7. **Preserve borrower payment instructions** and continuity of servicing.
8. **Notify** affected borrowers, any investors (Horizon 2), and relevant regulators.

## 4. What capitalYA MAY / MAY NOT do

| MAY | MAY NOT |
|---|---|
| Provide servicing **technology** and records | **Originate or approve** new loans |
| Coordinate the transition & communications | **Receive principal/repayments** into its accounts |
| Deliver borrower/loan data to the backup servicer | Set terms, restructure, or make credit decisions |
| Preserve audit trail and payment instructions | Represent itself as the lender/creditor |
| Facilitate regulator/borrower notifications | Continue the lender's **regulated** activities |

## 5. Backup servicer

- **Identified and vetted in advance** (name + trigger recorded in the agreement — open item `06` §13 / `10` §13).
- Receives complete, current borrower/loan files (deliverable format pre-agreed).
- Assumes legal servicing of record; capitalYA provides technology and data continuity only.
- Warm-standby expectations and a periodic file-handoff **dry run** during the pilot (`06`).

## 6. Data, records & funds continuity

- Borrower/loan files exported in the pre-agreed format; encryption and access controls preserved (`13`).
- Payment instructions and reconciliation history preserved (`06` §8); capitalYA's status display continues from the successor's records.
- **No capitalYA-held funds exist to transfer** — by design (`05`), there is nothing to unwind on the funds side at capitalYA.
- Data-protection obligations (retention/deletion, DSAR) continue through wind-down (`13`).

## 7. Communications

- Borrower notice (Spanish): who now services the loan, where to pay, that terms are unchanged, and complaint channels (`15`).
- Regulator notice as required (coordinated with counsel).
- Investor notice (Horizon 2 only).

## 8. Testing

The pilot includes a **lender-failure / servicing-continuity test** (`03_...` §15 Sep pilot; `01` §10): exercise triggers, backup-servicer handoff, file delivery, and notifications end-to-end.

## 9. Open items → agreement / counsel

`[Backup-servicer identity & contract]`, `[which loan assignments are legally permitted and how perfected]`, `[approved collection account/trustee]`, `[regulator notification requirements]`, `[file-handoff format & timing]`.
