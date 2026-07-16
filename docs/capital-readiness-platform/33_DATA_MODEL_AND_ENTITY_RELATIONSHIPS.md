# 33 — Data Model and Entity Relationships

| Field | Value |
|---|---|
| Purpose | Logical data model: entities, relationships, and integrity rules underpinning all services |
| Audience | Backend engineers, data engineers |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Backend lead |
| Dependencies | 34 (dictionary), 14 (states), 35 (storage), 36 (events) |
| Source references | CRF §16 layer 4 (entity/ownership graph), §20.1 (unique IDs); D-03…D-08 |
| Assumptions | Relational core (PostgreSQL-class) + object storage for files; graph queries via relational adjacency (no dedicated graph DB in MVP) |
| Open questions | — |
| Approval required | Backend lead |
| Last updated | 2026-07-16 |

## 1. Design rules

1. Unique durable IDs for borrower, entity, UBO, facility/request, collateral asset, contract, document, evidence item (CRF §20.1) — UUIDv7, never reused.
2. Multi-tenant scoping: every row carries `org_id` (or reaches one via mandatory FK path); enforced at data layer (NFR-22).
3. Soft state, hard history: current state on the row + full transition history in `workflow_transitions`; nothing is destructively updated where audit relevance exists (35).
4. All monetary values `{amount decimal(18,2), currency}`; all timestamps UTC.

## 2. Entity registry

### Identity & access
| ID | Entity | Key fields | Relationships |
|---|---|---|---|
| ENT-01 | Organization | type (borrower/internal/provider/partner/auditor), legal_name, registry_id, jurisdiction, status | has Users via Memberships |
| ENT-02 | User | person identity, auth ref, locale, MFA status | Memberships → Org+Role |
| ENT-03 | Membership | org, user, role (R-xx), status, invited_by | segregation checks read this |
| ENT-04 | AccessGrant | grantee org/user, resource (opportunity/workspace/section), scope, expiry, granted_by | provider/partner/auditor scoping (12) |

### Borrower domain
| ID | Entity | Key fields | Relationships |
|---|---|---|---|
| ENT-05 | BusinessProfile | overlay-driven identity fields, sector, size, description | 1:1 borrower Org |
| ENT-06 | PartyNode | kind (person/entity), name, ids, jurisdiction | ownership graph node; UBO = person node with control attrs |
| ENT-07 | OwnershipEdge | from-node, to-node, pct, instrument, source evidence | graph (cycle-checked); related-party edges typed |
| ENT-08 | CapitalRequest | amount, currency, purpose/module, UoP lines, repayment source (structured), tenor, collateral offered, version | 1:n per workspace (amendments versioned) |
| ENT-09 | ChecklistItem | evidence_type (EV-xx), status, required|conditional|optional, period spec, due | generated per profile (24) |
| ENT-10 | Task | source ref, assignee, due, state | FR-WS-05 |

### Evidence & documents
| ID | Entity | Key fields | Relationships |
|---|---|---|---|
| ENT-11 | Document | file ref (35), hash, uploader, declared/classified type, state (14 §4), version chain, privacy class, sharing state | n:1 ChecklistItem; versions self-ref |
| ENT-12 | Extraction | document, schema+bundle versions, envelope JSON, status | 1:n per document (per model version) |
| ENT-13 | EvidenceField | field path, value, provenance (30 §6), verification status/by/at | authoritative field store |
| ENT-14 | EvidenceLink | control (A1..H5), evidence (document/field), relevance weight | feeds 22 CCS |
| ENT-15 | Exception | type (contradiction/staleness/schema/defect), members (field/doc refs), materiality, status, resolution | never averaged (CRF App B) |

### Assessment & scoring
| ID | Entity | Key fields | Relationships |
|---|---|---|---|
| ENT-16 | Assessment | workspace, state (14 §3), model+overlay versions, profile id, validity/expiry | 1:n per borrower |
| ENT-17 | ControlAssessment | assessment, control, maturity/NA+code, assessor, evidence refs, AI-suggested value | unique (assessment, control) |
| ENT-18 | GateStatus | assessment, gate id, status, disposition ref | |
| ENT-19 | Flag | per 23 §4 structure incl. visibility + restricted | links controls/evidence/gates |
| ENT-20 | Override | target, system/override values, reason code, evidence, approver, expiry, status | maker≠checker enforced |
| ENT-21 | AssessmentResult | result version, system/published totals, dimension scores, contributions, caps triggered, confidence grades (per level), publication eligibility, input hash | immutable versions |
| ENT-22 | ExternalRiskContext | level 1–5, factors, mitigants, source notes | 1:1 per assessment |
| ENT-23 | RemediationPlan / ActionItem | per 28 | 1:1 per result |

### Consent, publication, providers
| ID | Entity | Key fields | Relationships |
|---|---|---|---|
| ENT-24 | ConsentRecord | subject org, scope (processing/screening/sharing:target), basis, granted_by, granted/revoked at, text version | consent ledger (GATE-05) |
| ENT-25 | Opportunity | assessment ref, state (14 §5), disclosure set, package artifact refs | 1:n AccessGrants |
| ENT-26 | RFI | opportunity, provider org, thread (messages, attachments via approved room), status | |
| ENT-27 | EOI | opportunity, provider org, indicative terms, non-binding ack, borrower response | |
| ENT-28 | ProviderProfile | mandate (sectors, amounts, structures, jurisdictions), agreement refs, diligence status | 1:1 provider Org |
| ENT-29 | OutcomeRecord | opportunity, outcome taxonomy (37), reported_by, date | pilot metrics/calibration |

### Platform
| ID | Entity | Key fields | Relationships |
|---|---|---|---|
| ENT-30 | AuditEvent | per 36 (append-only, hash-chained) | references any entity |
| ENT-31 | ScoringModelVersion | bundle JSON (controls/weights/profiles/caps/bands/thresholds), semver, status, changelog, approvals | pinned by ENT-16/21 |
| ENT-32 | JurisdictionOverlayVersion | overlay bundle (63 schema), country, semver, counsel-source notes | pinned by ENT-16 |
| ENT-33 | NotificationRecord | template+version, recipient, channel, sent/failed | |
| ENT-34 | Note | scope (internal/provider), author, body, object ref | visibility per 12 |
| ENT-35 | LegalHold | scope, reason, placed_by, released_by | blocks deletion (35) |
| ENT-36 | ReportArtifact | type (borrower/internal/provider), render version, hash, storage ref | 27 |

## 3. Key relationship diagram (textual)

```
Org(borrower) 1—1 BusinessProfile
             1—* Membership *—1 User
             1—1 Workspace 1—* ChecklistItem 1—* Document 1—* Extraction
                             |                    Document *—* EvidenceLink *—1 Control
                             1—* CapitalRequest(versioned)
                             1—* Assessment 1—* ControlAssessment
                                        1—* GateStatus / Flag / Override
                                        1—* AssessmentResult 1—1 RemediationPlan
                                        1—1 ExternalRiskContext
Assessment(approved) 1—* Opportunity 1—* AccessGrant(provider)
Opportunity 1—* RFI / EOI / OutcomeRecord
PartyNode *—* OwnershipEdge (graph, scoped to borrower org)
Everything —* AuditEvent (append-only)
```

## 4. Integrity rules (enforced)

Ownership edges acyclic; Σ direct ownership per node ≤100±0.5; ControlAssessment only for controls active in the pinned profile; AssessmentResult recompute never mutates prior versions; Flag with restricted=true requires co-sign records; AccessGrant beyond assessment expiry invalid (14 §6); ConsentRecord required before any provider AccessGrant on borrower data; document hash immutable post-write; EOI requires active provider agreement ref.
