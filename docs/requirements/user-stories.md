# User Stories

Format: *As a [role], I want [goal] so that [benefit].*
Acceptance criteria use **Given / When / Then** (Gherkin style).

---

## US-01 — Register a customer
**As a** support agent, **I want** to register a new customer **so that** they can open claims.

**Acceptance criteria**
- **Given** valid data, **when** I send `POST /api/v1/customers`, **then** the customer is created and I get `201 Created`.
- **Given** a document number that already exists, **when** I try to create the customer, **then** I get `409 Conflict`.
- **Given** an invalid email, **when** I send the request, **then** I get `400 Bad Request` with the field error.

**Story points:** 3 · **Requirement:** FR-01

---

## US-02 — Open a claim
**As a** customer, **I want** to open a claim describing my problem **so that** the company can solve it.

**Acceptance criteria**
- **Given** an existing customer and service, **when** I send `POST /api/v1/claims`, **then** the claim is created in status `OPEN` with a number like `CLM-2026-000123`.
- **Then** the priority and SLA due date are calculated automatically.
- **Given** the customer already has 3 open claims for the same service and category, **then** I get `422 Unprocessable Entity` (BR-03).

**Story points:** 5 · **Requirements:** FR-02, FR-03, FR-04

---

## US-03 — Assign a claim to an agent
**As a** supervisor, **I want** to assign an open claim to an agent **so that** someone is responsible for it.

**Acceptance criteria**
- **Given** a claim in `OPEN`, **when** I assign an agent of the correct department, **then** the status changes to `ASSIGNED`.
- **Given** an agent from another department, **then** I get `422` (BR-02).
- **Then** the change is saved in the status history.

**Story points:** 3 · **Requirements:** FR-06, FR-08

---

## US-04 — Change claim status
**As an** agent, **I want** to update the status of my claims **so that** the customer and my supervisor know the progress.

**Acceptance criteria**
- **Given** a valid transition (see state diagram in the SRS), **when** I send `PATCH /api/v1/claims/{id}/status`, **then** the status is updated.
- **Given** an invalid transition (e.g. `OPEN` → `CLOSED`), **then** I get `409 Conflict` with a clear message.
- **Then** each change is stored in `claim_status_history`.

**Story points:** 5 · **Requirements:** FR-07, FR-08

---

## US-05 — Calculate SLA with business days
**As a** supervisor, **I want** the SLA due date to skip weekends and national holidays **so that** deadlines are fair and realistic.

**Acceptance criteria**
- **Given** a HIGH claim opened on a Friday before a holiday Monday, **then** the due date skips Saturday, Sunday and Monday.
- **Given** the holidays API is down, **then** the system uses weekdays only and logs a warning (NFR-07).
- Holidays are cached per year to avoid repeated calls.

**Story points:** 5 · **Requirement:** FR-05

---

## US-06 — Search claims
**As a** supervisor, **I want** to filter claims by status, category, department and dates **so that** I can manage the workload.

**Acceptance criteria**
- `GET /api/v1/claims?status=OPEN&department=BILLING&from=2026-01-01&page=0&size=20` returns a paginated result.
- Results are ordered by SLA due date ascending by default.

**Story points:** 3 · **Requirement:** FR-10

---

## US-07 — Comment on a claim
**As an** agent or customer, **I want** to add comments **so that** we can communicate about the problem.

**Acceptance criteria**
- `POST /api/v1/claims/{id}/comments` creates a comment with author and date.
- Comments cannot be added to `CLOSED` or `REJECTED` claims (BR-04).

**Story points:** 2 · **Requirement:** FR-09

---

## US-08 — Management reports
**As a** supervisor, **I want** reports about claims **so that** I can make decisions based on data.

**Acceptance criteria**
- `GET /api/v1/reports/claims-by-status`
- `GET /api/v1/reports/avg-resolution-time` (per department, in hours)
- `GET /api/v1/reports/top-categories?limit=5`
- `GET /api/v1/reports/sla-breaches`
- Each report is backed by a SQL query documented in `database/queries/`.

**Story points:** 8 · **Requirement:** FR-11

---

## US-09 — Rate the solution
**As a** customer, **I want** to rate the solution of my closed claim **so that** the company can measure service quality.

**Story points:** 2 · **Requirement:** FR-14
