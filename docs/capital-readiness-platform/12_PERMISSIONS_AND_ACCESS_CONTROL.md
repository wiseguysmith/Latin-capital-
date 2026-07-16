# 12 — Permissions and Access Control

| Field | Value |
|---|---|
| Purpose | Authoritative permission matrix, sharing/consent rules, segregation of duties, and account lifecycle controls |
| Audience | Engineering (authz implementation), operations, security |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Security lead + Head of Product |
| Dependencies | 11 (roles), 43 (technical authz), 36 (audit) |
| Source references | Master prompt §9; CRF §15 stage 5, §16 layer 10 |
| Assumptions | ASM-12 (provider access expiry 90 days, watermarking) |
| Open questions | — |
| Approval required | Security lead |
| Last updated | 2026-07-16 |

## 1. Model

RBAC + resource scoping: `permission = role ∩ organization type ∩ resource scope (workspace / opportunity / assignment) ∩ object-level rules (document class, note type, flag visibility)`. Deny by default. Enforcement server-side in the authorization service (43); UI hiding is never the control.

## 2. Core permission matrix

Legend: C create, R read, U update, D delete/disable, A approve, ✱ scoped to own org/workspace, † scoped to explicit assignment/authorization. Blank = no access.

| Resource | R-PA | R-BA | R-BM | R-IA | R-AR | R-FR | R-LR | R-CR | R-SA | R-CPA | R-CPN | R-LP/CP/AP | R-AUD | R-SUP |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Preliminary application | CRU✱ | R✱ | | R | RUA | R | R | R | R | | | | R† | R(meta) |
| Borrower org & members | | CRUD✱ | R✱ | CRUD | | | | | | | | | R† | R(meta) |
| Company profile / capital request | | CRU✱ | RU✱(assigned) | R | R | R | R | R | R | R† (published view) | R† | R† | R† | |
| Ownership & UBO data | | CRU✱ | | R | R | R | R | RU(verify) | R | R†(summary) | R†(summary) | R†(assigned) | R† | |
| Documents (upload/versions) | | CRU✱ | CRU✱(assigned) | R | R | RU(verify) | RU(verify) | RU(verify) | R | R†(approved room) | R†(approved room) | CR†(assigned) | R† | |
| Document deletion | | request only | | A(policy) | | | | | A | | | | | |
| Clarification requests | | RU✱(respond) | RU✱(assigned) | R | CR | CRU | CRU | CRU | R | via RFI only | | CR†(assigned) | R† | |
| Control assessments (maturity) | | R✱(approved view) | | R | | CRU(A,B,D,G,H) | CRU(C,E) | CRU(F) | RA | | | propose† | R† | |
| Gates / red flags | | R✱(visible subset) | | R | CR | CRU | CRU | CRU + restricted | RA | R†(disclosed subset) | R†(disclosed subset) | R†(assigned) | R† | |
| Overrides | | | | R | | propose | propose | propose | A | | | | R† | |
| Assessment approval | | | | | | | | | A | | | | R† | |
| Readiness score/report | | R✱(approved) | R✱(approved) | R | R | R | R | R | RA | R†(published) | R†(published) | R†(assigned) | R† | |
| Opportunity packaging | | consent only | | CRU | | | | R | A | | | | R† | |
| Publication / provider grants | | A(consent) | | CRU | | | | R | A | | | | R† | |
| RFI | | RU✱(respond) | | R(route) | | R | R | R | R | CRU† | CR†(draft) | | R† | |
| EOI | | R✱ | | R | | | | | R | CRU† | R† | | R† | |
| Provider internal notes | | | | | | | | | | CRUD✱ | CRU✱ | | R†(if in scope) | |
| Internal notes (platform) | | | | CRU | CRU | CRU | CRU | CRU | CRU | | | | R† | |
| Audit log | | | | R | | | | R(compliance events) | R | | | | R† | |
| Scoring model config | | | | RU(draft) | | R | R | R | A(publish) | | | | R† | |
| Jurisdiction overlay config | | | | RU(draft) | | | R | R | A(publish) | | | | R† | |
| User support metadata | | | | R | | | | | | | | | | R |

Notes:
- **Internal vs external notes** are distinct entities (33); internal notes never render in borrower/provider surfaces; provider notes never render to borrowers or other providers.
- **Restricted flags** (legally/investigatively hidden, D-12): visible only to R-CR, R-SA, R-IA, and R-AUD; excluded from borrower and provider surfaces and from exports (23 §6).
- Borrower sees **approved** assessment results only (ASM-16).

## 3. Document-level access

Each document has: `privacy_class` (P1 public-ish corporate, P2 commercial-sensitive, P3 financial-sensitive, P4 PII/UBO-sensitive; 44 §3) and `sharing_state` (`internal_only`, `approved_room`, `restricted`). Provider room membership grants access only to `approved_room` docs; P4 docs enter approved rooms only as redacted summaries unless R-SA approves full sharing with borrower consent. Every provider view/download is an audit event with watermarking on download (ASM-12).

## 4. Consent rules (borrower-controlled sharing)

1. Publication to any provider requires an active consent record naming the provider org (or an explicit "any approved provider of type X" scope), signed by R-BA (CRF §19.2).
2. Consent is versioned and revocable; revocation removes access within 1 hour (NFR-09) but does not claw back lawfully retained audit copies (35).
3. RFI responses that add new documents to the approved room require R-BA action (implicit consent by the act of sharing, still logged).

## 5. Account lifecycle

| Event | Rule |
|---|---|
| Invitation | R-BA invites R-BM (borrower); R-IA invites internal/partner/auditor; R-CPA invites R-CPN. Email invite, expiring 7 days, role fixed at invite, acceptance = account activation. MFA enrollment required before first sensitive read (43). |
| Suspension | R-IA may suspend any external user/org (with reason, audited); suspension freezes access immediately, preserves data. Borrower org suspension pauses assessment lifecycle (14). |
| Removal | Removing a user revokes sessions ≤5 min; their historical actions remain attributed in audit. R-BA removal requires promotion of another R-BA first (an org must always have ≥1 R-BA). |
| Dormancy | External accounts idle >180 days are auto-suspended pending re-verification. |

## 6. Segregation of duties (mandatory, enforced by authz where systematizable)

1. The reviewer who proposes a control assessment cannot be the senior approver of the same assessment.
2. The proposer of an override can never approve it (maker-checker; CRF §18).
3. R-AR who accepted an applicant may not be the sole verifier of that applicant's identity evidence.
4. Scoring-model config: drafted by R-IA, published only by R-SA, never same person within one change.
5. Sanctions disposition requires R-CR; if R-CR raised the underlying flag, a second R-CR or R-SA must disposition.
6. Billing/fee roles (manual in MVP) must not hold R-FR/R-SA on the same accounts (D-15).
7. With ASM-02's small team, conflicts are resolved by explicit conflict declarations + R-SA reassignment; the system blocks self-approval combinations regardless of team size.

## 7. Reviewer conflicts & emergency access

- **Conflict declaration:** any internal reviewer or partner must declare a conflict (ownership, family, prior employment, financial interest) on assignment; conflicted users are excluded from that workspace's queues by R-IA.
- **Emergency access ("break-glass"):** R-IA + R-SA joint approval grants time-boxed (≤24h) elevated access with mandatory reason; every break-glass session is flagged in the audit log and reviewed by R-AUD scope monthly (44 §8).
