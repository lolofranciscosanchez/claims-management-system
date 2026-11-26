# Sprint 0 — 18/11/2026 to 26/11/2026

## Sprint goal
> Requirements, process models and data model agreed before coding.

**Result:** goal achieved. All committed items were completed; the sprint was closed one day after the planned end date (25/11).

## Committed stories
| Jira key | Story | Points | Status |
|---|---|---|---|
| CMS-10 | Set up Jira project and Confluence space | 1 | Done |
| CMS-11 | Write Software Requirements Specification | 3 | Done |
| CMS-12 | Write user stories with acceptance criteria | 2 | Done |
| CMS-13 | Model claim handling process in BPMN 2.0 | 5 | Done |
| CMS-14 | Draw flowchart: open claim use case | 2 | Done |
| CMS-15 | Draw flowchart: SLA due-date algorithm | 2 | Done |
| CMS-16 | Design and review Entity-Relationship Diagram | 3 | Done |

**Committed:** 18 pts · **Completed:** 18 pts

## Sprint review

### What was delivered
- **Requirements:** [SRS](../../requirements/software-requirements-specification.md) with 14 functional requirements, non-functional requirements, business rules BR-01 to BR-07 and a complete traceability matrix.
- **User stories:** [10 user stories](../../requirements/user-stories.md) with Given/When/Then acceptance criteria.
- **Process models:** [BPMN 2.0 model](../../process/claim-handling-process.svg) of the end-to-end claim process and two flowcharts ([open claim](../../process/flowchart-open-claim.png), [SLA due date](../../process/flowchart-sla-calculation.png)).
- **Data model:** [ERD](../../database/erd.md) with design decisions, indexes and constraints.
- **Tooling:** Jira Scrum board with the full backlog (8 epics, 32 items), Confluence space, GitHub repository with CI.

### Changes and decisions made during the sprint
| Topic | Decision | Where |
|---|---|---|
| Claim status | Status history with validity periods (`started_at` / `ended_at`) and a `STATUS` lookup table, instead of storing the status in `CLAIM`. | ERD, SRS FR-08, BR-06 |
| Traceability gap | FR-13 (automatic closing) had no user story; marked as out of scope for v1.0 (Could priority). | SRS section 6 |
| Story size | US-08 (8 pts) split into US-08a (3 pts, CMS-33) and US-08b (5 pts, new item CMS-41). | User stories, Jira |
| Service ownership | New rule BR-07: a claim can only reference a service that belongs to the customer and is not cancelled. | SRS, US-02, open claim flowchart |
| SLA calculation | Business hours = the 24 hours of each business day (weekends and national holidays excluded). | SRS definitions, SLA flowchart |
| Wording | Brand-neutral description of the company ("internet, mobile and TV service provider"). | README, SRS |
| Build | CI fixed (Maven Central rate limit) and Maven Wrapper added. | `.github/workflows/ci.yml` |

### Evidence
<!-- Add the screenshots to ./img/ and keep these names, or update the links. -->
- Jira board at the end of the sprint: `![Sprint 0 board](./img/sprint-0-board.png)`
- Jira sprint report: `![Sprint 0 report](./img/sprint-0-report.png)`

## Retrospective
<!-- Your own reflection. Guiding questions:
     - What helped you move forward? (tools, decisions, the way you worked)
     - What slowed you down or took longer than expected? Why?
     - What would you do differently in Sprint 1?
     - Each action item should be concrete and checkable at the end of the next sprint. -->

| What went well | What to improve | Action items |
|---|---|---|
| | | |
| | | |
| | | |
