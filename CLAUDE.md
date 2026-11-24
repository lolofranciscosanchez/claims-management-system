# CLAUDE.md — Instructions for AI coding agents

## Project context
Claims Management System: a Spring Boot REST API for an internet, mobile and TV service provider to register, assign, track and resolve customer claims. It is a **portfolio project** for an IT internship. The human owner must be able to explain every line in a job interview, so **clarity beats cleverness**.

Read before working:
- `docs/requirements/software-requirements-specification.md` (FR, NFR, business rules)
- `docs/requirements/user-stories.md` (acceptance criteria)
- `docs/database/erd.md` (data model)
- `docs/agile/working-agreement.md` (Git workflow, Definition of Done)

## Language
**Everything in English**: code, identifiers, comments, commit messages, PR descriptions, documentation.

## Tech stack
Java 21 · Spring Boot 3.5 · Maven (via wrapper) · PostgreSQL 16 (Docker) · Flyway · Spring Data JPA · springdoc-openapi · JUnit 5 + Mockito · H2 (tests only).

## Commands
```bash
docker compose up -d        # start PostgreSQL
./mvnw spring-boot:run      # run the API -> http://localhost:8080/swagger-ui.html
./mvnw test                 # unit + integration tests
./mvnw verify               # full build (same as CI)
```
Use the Maven Wrapper (`./mvnw`, or `mvnw.cmd` on Windows); it pins Maven 3.9.11, so no local Maven install is needed.

## Architecture rules
- Package by feature under `io.github.lolofranciscosanchez.claims`: `customer`, `claim`, `catalog`, `report`, `holiday`, `common` (errors, config).
- Layers: `Controller` → `Service` → `Repository`. Business rules **only** in services.
- Controllers receive/return **DTOs** (Java `record`s), never JPA entities.
- Validation with Jakarta Bean Validation on request DTOs.
- Errors through a single `@RestControllerAdvice` with the JSON format defined in NFR-06.
- The schema is owned by **Flyway** (`src/main/resources/db/migration`). Never use `ddl-auto=update`. Never edit an applied migration; create a new `V{n}__description.sql`.
- Every endpoint documented with `@Operation` and `@ApiResponse`.
- API base path: `/api/v1`.

## Working agreement for agents
1. Work on **one Jira ticket at a time** (the human gives you the key, e.g. `CMS-114`).
2. Create a branch `feature/CMS-<n>-short-description`.
3. Make small commits, each message starting with the key: `CMS-114: add claim status state machine`.
4. Write or update tests for every change; `./mvnw verify` must pass.
5. Update docs affected by the change (README, SRS, ERD).
6. Finish with a short summary for the human: what changed, why, how to test it in Swagger, and **concepts worth studying** for the interview.
7. Tickets labelled `hands-on` in `jira/backlog.csv` are done by the human: you may explain, review or give hints, but do not write the full solution unless explicitly asked.
8. Do not add dependencies without explaining why.
