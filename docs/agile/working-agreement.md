# Agile Working Agreement

The project follows **Scrum** adapted to a one-person team (with an AI coding assistant as "pair programmer").

## Cadence
| Item | Value |
|---|---|
| Sprint length | 1 week |
| Sprint planning | Monday – select stories from the backlog, define the sprint goal |
| Daily check-in | Short note in the Jira ticket or Confluence page (yesterday / today / blockers) |
| Sprint review | Sunday – demo of the working endpoints through Swagger UI / Postman |
| Retrospective | Sunday – what went well, what to improve, action items |

## Tools
| Tool | Usage |
|---|---|
| **Jira** (Free plan) | Product backlog, sprint board, burndown chart. Project key: `CMS`. |
| **Confluence** (Free plan) | Architecture notes, meeting notes, sprint reviews and retrospectives. |
| **GitHub** | Source code, pull requests, CI (GitHub Actions). |

## Git workflow
- `main` is always deployable; CI must pass.
- One branch per Jira ticket: `feature/CMS-12-create-claim-endpoint`, `fix/CMS-30-sla-weekend-bug`.
- Commit messages start with the Jira key: `CMS-12: add POST /claims endpoint`.
- Each branch is merged through a pull request that references the ticket.

## Definition of Ready (a story can enter a sprint when…)
- [ ] It has a clear user story and acceptance criteria.
- [ ] It is estimated in story points.
- [ ] Dependencies are identified.

## Definition of Done (a story is finished when…)
- [ ] Code follows the layered architecture and compiles without warnings.
- [ ] Unit tests written and passing; CI green.
- [ ] Endpoint documented in Swagger (summary, description, response codes).
- [ ] Acceptance criteria verified manually (Postman collection updated).
- [ ] Related documentation updated (README / SRS / ERD).
- [ ] Pull request merged and Jira ticket moved to **Done**.
- [ ] Related documents checked for consistency (SRS ↔ user stories ↔ diagrams ↔ ERD).

## Estimation
Fibonacci story points: 1, 2, 3, 5, 8. Anything bigger than 8 must be split.
