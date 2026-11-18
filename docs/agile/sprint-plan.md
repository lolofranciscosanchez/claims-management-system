# Release Plan (v1.0)

| Sprint | Goal | Main tickets |
|---|---|---|
| **Sprint 0** – Analysis & design | Requirements, process models and data model agreed before coding. | SRS, user stories, BPMN, flowcharts, ERD, Jira/Confluence setup |
| **Sprint 1** – Data layer | Database schema versioned and populated with realistic test data; analytical SQL queries written. | Flyway migrations, seed data, SQL reports, JPA entities |
| **Sprint 2** – Core API | Customers can open claims and supervisors/agents can manage them through the REST API. | Customers API, Claims API, state machine, history, error handling, Swagger |
| **Sprint 3** – Integration & reports | SLA uses real Argentine holidays and supervisors get management reports. | Holidays API client, SLA calculation, report endpoints, Postman collection |
| **Sprint 4** – Quality & release | Tested, documented and easy to run. | Unit/integration tests, README in English, demo, v1.0 tag |

Each sprint is documented in `docs/agile/sprints/` using the template.
