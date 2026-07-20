# 20 — Build-Phase Plan (Engineering Roadmap to the Pilot)

| Field | Value |
|---|---|
| Purpose | Turn the documentation program into working software — recommended stack, repo structure, boundary-as-code enforcement, and a sequenced build to the Costa Rica closed pilot |
| Audience | Engineering leads, founders, technical hires, fractional CTO/contractors |
| Status | Draft v1.0 — **do not start production code until the product owner approves the doc suite and counsel validates the boundary model** (CRP `00` §6; `03_...` §17) |
| Version | 1.0.0 |
| Owner | Founder + engineering lead |
| Dependencies | Screen specs `19` + CRP `18`; schemas `07`; boundary `05`; integration `06`; CRP technical set `40`–`47`, security `44`, identity `43`, environments `46`, QA `73`; mockups `mockups/` |
| Governing constraint | Every build decision inherits the prohibitions in `05` and the invariants in `00` §6. Boundary is enforced in code, not just policy. |
| Last updated | 2026-07-19 |

---

## 1. Reality check on the timeline

Per the launch calendar (`03_...` §15; `01` §10), the closed pilot targets **September 2026**, and code should not begin until **counsel validates the model (August)** and a **partner lender signs**. That leaves ~4–6 weeks to a live pilot — **not enough to build the fully automated platform.**

So the pilot is a **thin vertical slice with humans in the loop**, exactly as CRP principle P9 ("manual-first — automate only after it works manually") intends. The SOPs (CRP `51`–`56`) are written to run on spreadsheets + a document vault alone. The build below front-loads the irreducible software (identity, consent, documents, audit, the lender portal) and lets the assessment run **analyst-operated** for the pilot, automating it after.

**Design the schema and boundaries for the full system now; implement the pilot slice first.**

## 2. Build principles (inherited, non-negotiable)

1. **Boundary-as-code.** Every `05` prohibition becomes a technical control (§7), not a policy hope.
2. **Deterministic core owns all math.** Scoring, gates, caps, state transitions run in deterministic services; **AI only extracts/proposes and never decides** (CRP `02` P3, `31`).
3. **Audit-first.** Every material action writes an immutable audit event before the UI confirms it (CRP `36`).
4. **Config, not constants.** Control library, fees, and lender checklists ship as **versioned configuration** (CRP `00` §6.3; `08`; `07` §4.6) — never hardcoded.
5. **No funds rails, ever.** There is no money-movement code path in the system (§7).
6. **Consent gates data flow** at the service layer, not the UI layer.
7. **Manual fallback exists** for anything not yet automated; the pilot can degrade to SOP + tooling without breaking the boundary.

## 3. Recommended stack

Opinionated default for a compliance-heavy, audit-first, document-centric MVP that must ship fast and read clearly to regulators. Vendor choices (KYC, e-sign, etc.) are governed by CRP `47` build-vs-buy.

| Layer | Recommendation | Why |
|---|---|---|
| **Web (borrower + lender portal + admin)** | **Next.js / React + TypeScript** | The mockups are already HTML/JS; React port is direct. One framework, three role-gated surfaces. SSR helps with the ES-first, accessible, auditable UI. |
| **API / orchestration** | **TypeScript (NestJS or Fastify)** | Shared types with the web app and the `07` schemas (generate types from JSON Schema / Zod). Strong DTO validation = boundary enforcement at the edge. |
| **Document intelligence / extraction** | **Python service (FastAPI)** | Extraction/verification (CRP `30`/`32`) is Python-native (OCR, PDF parsing, ML). Isolated service — assistive only, returns proposals to the deterministic core. |
| **Scoring engine** | **TypeScript module, pure + deterministic** | Owns the math (CRP `20`/`21`/`23`). Pure functions over versioned config; fully unit-testable; no AI, no I/O. Keep it a library, not a network hop. |
| **Datastore** | **PostgreSQL** | Relational integrity for entities (CRP `33`), JSONB for RAP/config, and an **append-only audit table** (triggers block UPDATE/DELETE). |
| **Object storage** | **S3-compatible, encrypted (SSE-KMS)** | Documents (CRP `35`); time-limited pre-signed URLs (`06` §7); US region permitted with consent (D-23). |
| **Auth / identity** | **OIDC (Auth0/Cognito/Keycloak) + RBAC**; **mTLS** for the phase-2 lender API | Roles per `06` §3 / CRP `43`; SSO-ready for lender staff. |
| **Async / events** | **Postgres-backed queue (pgboss) or SQS** | Event envelope (`07` §4.5); **idempotency keys** first-class (`06` §8.1). Don't over-engineer with Kafka for a pilot. |
| **Infra** | **One cloud (AWS preferred), IaC (Terraform), containerized** | CRP `46`; single region to start; reproducible environments. |
| **Observability** | **Structured logs + OpenTelemetry + error tracking** | CRP `45`; audit ≠ logs (audit is in Postgres, immutable). |

**Deliberately NOT in the stack:** any blockchain, wallet, token, oracle, DEX, or payment-initiation library (`05` §3; `07` §5). Their absence is a feature — a regulator can verify the system *cannot* do what we say it doesn't.

## 4. System decomposition

Maps to the service/module boundaries in CRP `40`/`41` and the two planes in `02`.

```
                    ┌──────────────── WEB (Next.js) ────────────────┐
                    │  borrower intake  │  lender portal │  admin    │
                    └────────┬──────────┴───────┬────────┴─────┬─────┘
                             │                  │              │
                    ┌────────▼──────────────────▼──────────────▼─────┐
                    │            API / ORCHESTRATION (TS)             │
                    │  auth · RBAC · consent gate · workflow/state    │
                    │  machine · handoff · idempotent events · audit  │
                    └───┬───────────┬──────────────┬────────────┬─────┘
                        │           │              │            │
             ┌──────────▼──┐ ┌──────▼──────┐ ┌─────▼─────┐ ┌────▼─────────┐
             │ SCORING     │ │ DOC-INTEL   │ │ DOCUMENTS │ │ POSTGRES     │
             │ engine (TS, │ │ (Python,    │ │ (S3+KMS,  │ │ entities +   │
             │ determinist)│ │ assistive)  │ │ presigned)│ │ append-only  │
             └─────────────┘ └─────────────┘ └───────────┘ │ audit + RAP  │
                                                            └──────────────┘
   External (via adapters, all optional for pilot): KYC/sanctions provider ·
   e-signature · email/SMS · (lender API — phase 2). NO funds/PSP integration.
```

## 5. Repository structure

Monorepo (pnpm/turbo), so shared `07` schemas stay single-source across web, API, and tests.

```
capitalya/
  packages/
    schemas/            # 07 canonical JSON Schemas + generated TS types + Zod validators (SINGLE SOURCE)
    scoring-engine/     # deterministic: controls, gates, caps, bands (20/21/23) — pure, versioned config
    config/             # control library, fee config (08/07 §4.6), lender checklists — versioned YAML/JSON
    ui/                 # shared React components, design tokens (from the mockups)
  apps/
    web-borrower/       # SCR-A/B intake journey (CRP 18 + borrower mockup)
    web-lender/         # SCR-D portal (19 + lender mockup)
    web-admin/          # internal review/approval console (CRP 18 admin)
    api/                # orchestration, workflow state machine, consent gate, audit, handoff
    doc-intel/          # Python FastAPI extraction/verification service (assistive)
  infra/                # Terraform, environment config (46)
  docs/ -> ../docs      # the program + CRP docs are the spec of record
  test/                 # traceability to CRP 72/73/79 acceptance + TC-### cases
```

## 6. Data & schema foundation (build first)

1. **Generate types + validators from `07`** in `packages/schemas` — RAP, capitalYA validation result, KYC object, consent record, event envelope, fee config. Everything downstream imports these; nothing redefines them.
2. **Core entities** per CRP `33`/`34` (borrower, application, document, assessment, consent, user/role, audit event, loan-status mirror).
3. **Append-only audit** (CRP `36`): DB triggers reject UPDATE/DELETE; every state transition and access writes an event with actor/role/timestamp/reason.
4. **Consent ledger** (`ENT-24`): scoped, timestamped, withdrawable; the consent gate reads it before any cross-plane data movement.
5. **Versioned config tables**: control library, fee config, lender checklist (`lender_policy_version`) — with effective dates.

## 7. Boundary-as-code: each prohibition → a control

The differentiator. Build these as tested invariants, not documentation.

| `05` prohibition | Technical control |
|---|---|
| Never hold/move funds | **No payment/PSP library in the dependency tree**; a CI check fails the build if one is added. No account/balance/transfer entities exist. |
| Never approve credit | Legally-meaningful states are writable **only** by lender-role tokens; the API rejects such transitions from any other principal (not hidden — absent). |
| capitalYA makes no credit decision | The validation-result object hard-sets `final_credit_decision = null`; type system forbids a non-null value. |
| Score ≠ approval/rating | RAP serializer **requires** the limitation-statement field; serialization fails without it. No amount/PD/price field exists on the RAP type. |
| No PII on-chain | No chain client exists; document/PII stores are Postgres/S3 only. |
| No automated sanctions clearance | No "clear match" endpoint exists; a potential match sets the escalation flag and blocks progression until a recorded lender outcome. |
| Consent-gated sharing | Handoff service calls the consent gate; missing/withdrawn scope → hard denial + audit event. |
| Payment status ≠ scoring input | Scoring engine has **no import path** to servicing/payment data; enforced by module boundaries + lint. |
| No prohibited terms | Copy-lint (CRP `73` §6) in CI over all UI strings. |

## 8. Build sequence

### Phase 0 — Foundations (Week 1–2, can start on green light)
Monorepo + CI/CD + IaC (one environment); `packages/schemas` from `07`; Postgres with entities + **append-only audit** + consent ledger; OIDC auth + RBAC roles (`06` §3); design tokens/components from the mockups. **Exit:** a logged-in user with a role, every action audited, schemas validating.

### Phase 1 — Pilot slice (Week 2–5) — *the September pilot runs on this*
The irreducible software; assessment is **analyst-operated** (SOP + admin tooling), not yet automated.
- **Borrower intake** (`web-borrower`): the mockup made real — application, **Ley 8968 consent capture** to the ledger, document upload to S3+KMS.
- **Admin review console** (`web-admin`): analysts run the CRP SOPs, record evidence/flags, and **produce a signed RAP** using the scoring engine over versioned config (human-approved, D-07).
- **Scoring engine** (`packages/scoring-engine`): deterministic controls/gates/caps/bands — even if analysts drive inputs manually at first.
- **Lender portal** (`web-lender`): SCR-D core flow (`19`) — queue → RAP viewer → KYC review → underwriting → **decision/terms/disbursement recording** (lender-only, audited) → servicing/reconciliation import (idempotent).
- **Consent gate + handoff** in the API; **boundary controls from §7 wired and tested.**
- **Reconciliation import** (CSV upload, idempotent) — no live lender API needed.
- **Assignment-notice dispatch** (`06` §8.2): on `disbursement.recorded` for factoring, generate + digitally sign + dispatch the *notificación de cesión* in the lender's name, store delivery evidence, emit `assignment.notice_dispatched`; failure = blocking incident. Statutory (Ley 9244) — pilot-required, counsel-approved template.
- **Fee-config validation**: `payer:"borrower"` rejected for mandatory fees (`21` D-P1) — part of the §7 boundary controls.

**Exit = the pilot launch gate:** a borrower can apply and consent; an analyst can produce a signed, human-approved RAP; the lender can review and record its own decisions through the portal; funds move entirely outside the system; every step is audited; §7 controls pass their tests.

### Phase 2 — Automation (post-pilot, Oct+)
Automate what the pilot ran manually: `doc-intel` extraction/verification (CRP `30`/`32`, assistive), automated evidence checks, notifications/tasks (CRP `19`), borrower remediation loop, richer analytics (CRP `37`). Optional phase-2 **lender API** (`06` §11) if the partner wants off portal-only.

### Phase 3 — Scale & Horizon-2 readiness (gated, later)
Second lender → matching/routing (deferred from MVP); El Salvador/Colombia expansion adapters; and **only behind `03_...` gates**, the tokenization stack (SPV/attestation/ZK) — none of which touches this codebase until counsel clears it.

## 9. Build vs. buy vs. defer

| Capability | Decision |
|---|---|
| KYC/identity/sanctions screening | **Buy** (provider per CRP `47`); we orchestrate + store results as `preliminary_non_relied` |
| E-signature (lender contract acceptance) | **Buy** |
| Document storage/OCR primitives | **Buy** infra (S3/KMS, OCR API); **build** the evidence/verification logic |
| Scoring engine, gates, caps | **Build** — core IP, must be deterministic + auditable |
| Consent ledger, audit, workflow state machine | **Build** — boundary-critical |
| Lender portal + borrower intake | **Build** — the differentiator; specs + mockups ready |
| Payments / funds movement | **Neither — never build or integrate** (`05`) |
| Tokenization / chain | **Defer** to Horizon 2, gated |

## 10. Environments, security, CI/CD

- **Environments:** local → staging → production, separate credentials/data, no prod PII outside prod (CRP `46`; `06` §3).
- **Security:** encryption in transit + at rest; secrets manager; least-privilege IAM; dependency scanning; the **§7 boundary controls as CI gates**; pen-test before launch (CRP `44`, `03_...` §15 security assessment).
- **CI/CD:** typecheck + unit (scoring engine has the deepest coverage) + schema-validation + copy-lint + boundary-control tests; traceability to CRP `72`/`73`/`79`.
- **Data protection:** DSAR + retention/deletion jobs (`13`); PRODHAB-analysis outcomes reflected in config.

## 11. Team shape & rough effort

Minimum to hit the pilot slice (Phase 0–1): **1 full-stack TS lead, 1 full-stack TS, 1 designer→frontend (mockups→React), 0.5 Python (doc-intel, can slip to Phase 2), 0.5 DevOps.** Compliance/PM from the founding team. Phase 0–1 is ~4–6 focused weeks with this team **if** specs are frozen (they are) and counsel/partner inputs land on schedule. Doc-intel automation (Phase 2) adds ~4–6 weeks.

## 12. Definition of done — pilot launch gate

Ship the pilot only when (aligns `09` §5 Stage 4, CRP `77`):
- [ ] Borrower can apply, consent (granular, Ley 8968), and upload documents.
- [ ] Analyst can produce a **signed, human-approved RAP** with the limitation statement.
- [ ] Lender can review and **record its own** KYC, decision, terms, disbursement, servicing — all lender-attributed and audited.
- [ ] **No funds path exists**; reconciliation import is idempotent.
- [ ] All §7 boundary controls pass automated tests.
- [ ] Security assessment passed; cyber/E&O bound (`03_...` §15).
- [ ] Counsel confirms the narrowed model; partner signed the responsibility matrix.
- [ ] ES operative text (counsel-approved) in every borrower-facing surface.

## 13. Explicit non-goals for the build

No investor UI, no tokens/wallets/oracles/DEX, no payment initiation, no pooled-capital logic, no capitalYA-side credit decisioning, no matching/routing (single lender), no multi-country logic beyond config seams. Any of these is a scope + boundary change requiring Founder + counsel sign-off (`05` §8). Build the compliant loan-and-assessment engine first; everything else waits behind a gate.
