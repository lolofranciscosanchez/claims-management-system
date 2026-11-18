# Jira backlog import

`backlog.csv` contains 8 epics and 31 stories/tasks (≈99 story points), ready to import into a **Jira Cloud Free** project.

## Labels
- `sprint-0` … `sprint-4`: suggested sprint (see `docs/agile/sprint-plan.md`).
- `hands-on`: done by me personally (process modeling, key SQL, core business logic).
- `agent-review`: implemented with an AI coding assistant and reviewed by me.

## How to import
1. Create a Jira project → **Scrum** template → key **`CMS`**.
2. ⚙ Settings → **System** → **External system import** → **CSV** (or, in team-managed projects, *Import work items* from the project's `...` menu).
3. Upload `backlog.csv`, encoding UTF-8.
4. Map fields: `Issue Id` → Issue Id, `Parent Id` → Parent Id, `Issue Type`, `Summary`, `Description`, `Priority`, both `Labels` columns → Labels, `Story Points` → Story point estimate.
5. Create Sprint 0 … Sprint 4 on the backlog and drag tickets according to their `sprint-n` label.
