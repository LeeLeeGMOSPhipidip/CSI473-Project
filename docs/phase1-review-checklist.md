# Remmogo Phase 1 Review Checklist

## Submission record

| Field | Value |
|---|---|
| Project | Remmogo |
| Team | 04 |
| Approval decision and date | Approved, 17 August 2026 |
| Repository | https://github.com/LeeLeeGMOSPhipidip/CSI473-Project |
| Submission tag | `phase1-submission` |
| Submission date | 11 September 2026 |
| Required PDF | `CSI473_Phase1_Team04.pdf` |

## Rubric-based review

| Criterion | Status | Evidence or action |
|---|---|---|
| Approved problem, stakeholders and scope | Complete | Approval record, problem evidence, stakeholder table, scope, assumptions and constraints are included in the report. |
| Functional requirements, actors, use cases and acceptance criteria | Complete | Requirements are numbered FR-01 to FR-14 and the core Request to Rent Item flow has measurable acceptance criteria. |
| Measurable quality requirements | Complete | Six project-specific scenarios define stimuli, operating contexts and response measures. |
| Domain model, business rules and responsibilities | Complete | The report includes the team domain model, business rules and CRC responsibility allocation. |
| Interaction and lifecycle modelling | Complete | Sequence, state-machine and activity models are included as supplied by the team. |
| Traceability and consistency rationale | Complete | Appendix A and `docs/consistency-matrix.md` link use-case steps, messages, responsibilities, states and evidence. |
| Focused review and issue closure | Complete | Three high-risk findings were recorded and the availability, payment-boundary and Rental-lifecycle wording was revised. |
| Reproducible submission and editable evidence | Team action required | Confirm that editable sources and readable PDF, SVG or PNG exports are committed and linked from the README. |
| Architecture drivers and alternatives | Complete | `docs/architecture-drivers.md` records three project-derived drivers and three realistic alternatives with consequences. |
| Report communication, references and integrity record | Complete | Metadata, references, review history and AI-use disclosure are included. |

## Focused review findings

### Finding 1 Availability during reservation

- **Risk:** An accepted request could remain visible as available and allow conflicting Rentals.
- **Correction:** The storefront requirement, business rule, state explanation and consistency matrix now exclude fully reserved quantity from availability.
- **Affected evidence:** FR-07, FR-11, duplicate-booking acceptance criterion, consistency matrix, architecture driver AD-01.
- **Status:** Closed.

### Finding 2 Payment boundary

- **Risk:** Wording could imply that Remmogo processes or guarantees payment.
- **Correction:** The report, requirements and architecture decision state that payment occurs externally and Remmogo records participant confirmations only.
- **Affected evidence:** FR-08, payment acceptance criterion, ADR-01, architecture alternative A.
- **Status:** Closed in the written baseline. Existing team diagrams are retained unchanged.

### Finding 3 Rental lifecycle focus

- **Risk:** Interface actions and Rental states could be confused, making lifecycle responsibilities unclear.
- **Correction:** The final lifecycle vocabulary and consistency explanation focus on PendingTransaction, Reserved, confirmation waiting states, ActiveRental, Overdue, Completed and Cancelled.
- **Affected evidence:** State-machine section, glossary, consistency matrix and Rental responsibility.
- **Status:** Closed.

## Repository readiness

- [ ] Final report saved as `submissions/CSI473_Phase1_Team04.pdf`.
- [ ] `docs/phase1-review-checklist.md` committed.
- [ ] `docs/architecture-drivers.md` committed.
- [ ] `docs/consistency-matrix.md` committed.
- [ ] Approval form and approval conditions retained.
- [ ] Editable use-case, domain, sequence, state-machine and activity sources retained.
- [ ] Readable exports retained for every model.
- [ ] README identifies the final report, model sources and exports.
- [ ] Dr Kombe can access the repository.
- [ ] All revised artefacts committed with a meaningful revision message.
- [ ] Annotated tag `phase1-submission` created and pushed.
- [ ] Tag checked on GitHub before Moodle upload.
- [ ] Only `CSI473_Phase1_Team04.pdf` uploaded to Moodle.


