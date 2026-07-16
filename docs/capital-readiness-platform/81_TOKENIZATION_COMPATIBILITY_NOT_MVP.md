# 81 — Tokenization Compatibility (Not MVP)

| Field | Value |
|---|---|
| Purpose | Document how the platform stays compatible with future tokenized financial assets without putting anything token-related on the MVP critical path |
| Audience | Architects, leadership, counsel |
| Status | Draft v1.0 — informational; no build commitment |
| Version | 1.0.0 |
| Owner | Head of Product + Backend lead |
| Dependencies | 33 (IDs/data standards), 63 (overlays), 64 (legal) |
| Source references | CRF §20 (principle + design table), §20.1 (data standards to build now); D-10; SV digital-asset separation (62 §2) |
| Assumptions | None beyond CRF §20 |
| Open questions | None open for MVP — all future |
| Approval required | n/a (informational) |
| Last updated | 2026-07-16 |

## 1. Governing principle (CRF §20)

> Tokenize a legally valid, serviced, enforceable financial claim — not a PDF, appraisal, or vague economic promise. The legal instrument, authoritative ownership record, payment rights, security package, and transfer restrictions must exist before the token layer.

The MVP builds **traditional-first**; tokenization is a possible future representation layer on top of the same disciplined data. Nothing in MVP scope may depend on, advertise, or imply token features (D-10; 02 §2.3).

## 2. What the MVP already does that keeps the door open (CRF §20.1 — build-now standards, all in scope via 33/34)

1. **Durable unique IDs** for borrower, entity, UBO, facility/request, instrument-precursor (capital request), collateral asset, contract, document, evidence item (33 §1).
2. **Canonical machine-readable terms**: capital-request and (future) closed-facility structures store currency, principal, rate/index, dates, amortization, covenants, events-of-default fields as data, not prose (ENT-08 + provider-package data spine 27 §2).
3. **Event model** naming discipline (36) extensible to draw/payment/delinquency/waiver/amendment/collateral-update/covenant-breach/acceleration/payoff events (post-MVP monitoring R1 introduces these).
4. **Field-level provenance + cryptographic hashes** of authoritative documents (30 §6; 27 §1; 35 §1).
5. **Role/permission model** mappable to issuer, administrator, servicer, custodian, transfer agent, investor, regulator (12's scoped-grant pattern generalizes).
6. **Standards compatibility posture**: LEI/GLEIF identifiers, BODS-style beneficial-ownership data shapes, verifiable-credential-friendly attestations, UNCITRAL MLETR awareness for electronic transferable records (CRF §17/§20.1, refs R18–R20).

## 3. Future extension map (CRF §20 design table, condensed)

| Element | Traditional-first (MVP posture) | Token-compatible extension (future) |
|---|---|---|
| Legal instrument | executed off-platform docs referenced by hash | machine-readable terms linked to token metadata |
| Register | provider/borrower records off-platform | on-chain register **only if legally recognized**; else synchronized authoritative off-chain register |
| Eligibility | manual/legal transfer restrictions | whitelist/credential checks, jurisdiction-aware transfer rules |
| Servicing | payment schedules + reconciliations (R1) | servicing oracle/events with independent reconciliation — never blind smart-contract reliance |
| Collateral | perfection via registries (H3) | token references security; **does not itself perfect a lien unless law provides** |
| Privacy | PII/confidential docs off-chain always | hashes/commitments, selective disclosure, verifiable credentials; no sensitive raw data on public chains |
| Custody/settlement | licensed partners, platform never touches funds (02) | regulated digital-asset/custody partners; platform remains orchestration layer |
| AML | KYC/KYB/UBO + monitoring | wallet screening + FATF Travel-Rule controls where applicable |
| Recovery | legal remedies, correction processes (57) | pause/freeze/burn-reissue/key-loss procedures governed by legal documents |

## 4. Guardrails until then

- No token/blockchain terminology in any user-facing surface or sales material (copy lint candidate list).
- El Salvador digital-asset regime stays operationally and legally separate from any lending workflow (62 §2 SV-L8; CRF §19.1).
- Any future tokenization initiative starts with: counsel memo per jurisdiction (64 G2), risk-committee approval, new decision-log entries, and its own documentation suite — it does not amend MVP boundaries by drift.
