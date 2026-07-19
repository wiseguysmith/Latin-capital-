# Consistency Review — Program Docs `00`–`18` (2026-07-19)

| Field | Value |
|---|---|
| Scope | All 19 program documents + `sources/` + the CRP README pointer |
| Method | Automated cross-reference verification (grep) + editorial pass |
| Result | **PASS — no contradictions found.** 2 intentional items noted, 0 fixes required |
| Reviewer | Program documentation pass (this session) |

---

## Checks performed

| # | Check | Result |
|---|---|---|
| 1 | Retired name "TITANO" appears nowhere outside `sources/` | ✅ Only 2 hits — both in `01` §11, which are the *deliberate* name-retirement notes |
| 2 | Brand spelling — `capitalYA` (308), `CAPITALYA` (12, all fee codes/JSON), `capitalya` (1, state name) | ✅ All lowercase/uppercase variants are code identifiers (`submitted_to_capitalya`, `CAPITALYA_SUCCESS_FEE`) — intentional conventions |
| 3 | Every doc-number reference `00`–`18` resolves to an existing file | ✅ All resolve; no dangling references to a doc `19`+ |
| 4 | Apparent "doc 19+" references are CRP-suite docs (`27`, `33`, `36`, `40`, `42`, …) | ✅ All prefixed "CRP" or pathed `../capital-readiness-platform/`; all exist |
| 5 | Section references into `03` (§15, §16, §17), `08` (§4, §5), `06` (§8.1, §9, §10, §13), `07` (§4.3, §4.4, §4.6) | ✅ All target sections exist |
| 6 | `05` §3 prohibition-item numbering vs. every `05 §3.N` citation (3.5 prohibited terms, 3.6 tokens/DEX, 3.8 sanctions/fraud automation, 3.10 payment-status firewall, 3.12 no-promotion-on-lender-failure) | ✅ All 10 citations map to the correct item |
| 7 | CRP assessment fee stated consistently ($2,500–$5,000 flat, non-contingent) across `01`, `03`, `04`, `10`, `12`, `17` | ✅ Consistent; `10`/`12`/`17` correctly mark it `[TBD]` within the range |
| 8 | Schema single-source rule (all canonical JSON in `07`; `06`/`08`/`17` reference, never redefine) | ✅ Held — `08` §6 and `17` §4 reproduce the `07` §4.6 example verbatim as instances, flagged as such |
| 9 | State machine (`06` §4) vs. event types (`07` §4.5) vs. callback table (`02` §6) | ✅ Aligned (`decision.recorded`, `terms.recorded`, `disbursement.recorded`, `servicing.event`) |
| 10 | Precedence rules identical in `00` §5 and `01` §12 | ✅ Consistent (03 governs near-term; 01 governs scope; CRP suite governs assessment; sources lowest) |
| 11 | CRP README pointer (`../capital-readiness-platform/00_README.md`) targets exist | ✅ |
| 12 | Every Tier-2 doc carries a draft/counsel-required status banner; `17`/`18` are number-free | ✅ |

## Intentional items (do not "fix")

1. **`01` §11 TITANO mentions** — these document the name retirement; removing them would erase the traceability.
2. **Lowercase/uppercase brand variants in code identifiers** — state names and fee codes follow code conventions, not prose branding.

## Residual risks (tracked, not doc defects)

- `05` §3 is cited by item number from six documents; renumbering that list is a **breaking change** — any future edit must preserve item numbers or update all citations (checked list: `06` §8.1, `07` §2/§5, `08` §2.1/§7, `11` §1/§4, `12` §4, `14` §4.1, `16` §1, `17` §6).
- Spanish operative texts do not exist yet anywhere; every customer-facing doc correctly flags this, but it remains the single largest content gap (counsel deliverable).
- The CRP suite still describes itself as standalone in docs `01`–`82` (by design — see `00` §1); only its README carries the program pointer. Acceptable for now; revisit after counsel validates the program layer.
