# Entity-Relationship Diagram 

> Draft logical model. Review it, adjust it to your own design decisions and then implement it as a Flyway migration (`src/main/resources/db/migration/V1__create_schema.sql`).

```mermaid
erDiagram
    CUSTOMER ||--o{ CUSTOMER_SERVICE : "has"
    SERVICE_TYPE ||--o{ CUSTOMER_SERVICE : "is of type"
    CUSTOMER ||--o{ CLAIM : "opens"
    CUSTOMER_SERVICE ||--o{ CLAIM : "affected by"
    CLAIM_CATEGORY ||--o{ CLAIM : "classifies"
    DEPARTMENT ||--o{ CLAIM_CATEGORY : "is responsible for"
    DEPARTMENT ||--o{ EMPLOYEE : "employs"
    EMPLOYEE |o--o{ CLAIM : "is assigned"
    CLAIM ||--o{ CLAIM_STATUS_HISTORY : "records"
    CLAIM ||--o{ CLAIM_COMMENT : "has"
    STATUS ||--o{ CLAIM_STATUS_HISTORY : "is used in"

    CUSTOMER {
        bigint id PK
        varchar document_number UK
        varchar full_name
        varchar email
        varchar phone
        timestamp created_at
    }
    
    SERVICE_TYPE {
        int id PK
        varchar name "MOBILE, INTERNET, TV, LANDLINE"
    }
    CUSTOMER_SERVICE {
        bigint id PK
        bigint customer_id FK
        int service_type_id FK
        varchar line_or_account_number UK
        varchar status "ACTIVE, SUSPENDED, CANCELLED"
        date start_date
    }
    DEPARTMENT {
        int id PK
        varchar code UK "TECH, BILLING, SALES"
        varchar name
    }
    CLAIM_CATEGORY {
        int id PK
        varchar code UK
        varchar name
        int department_id FK
        varchar default_priority
    }
    EMPLOYEE {
        bigint id PK
        varchar full_name
        varchar email UK
        varchar role "AGENT, SUPERVISOR"
        int department_id FK
        boolean active
    }
    CLAIM {
        bigint id PK
        varchar claim_number UK
        bigint customer_id FK
        bigint customer_service_id FK
        int category_id FK
        bigint assigned_employee_id FK "nullable"
        text description
        varchar priority "LOW, MEDIUM, HIGH, CRITICAL"
        timestamp sla_due_at
        smallint rating "nullable, 1-5"
    }
    CLAIM_STATUS_HISTORY {
        bigint id PK
        bigint claim_id FK
        int status_id FK
        timestamp started_at
        timestamp ended_at "nullable, null = current status"
        varchar changed_by
        varchar note
    }
    STATUS {
        int id PK
        varchar name
        varchar code "OPEN, ASSIGNED, REJECTED, IN_PROGRES, RESOLVED, PENDING_CUSTOMER, CLOSED"
        text description
    }
    CLAIM_COMMENT {
        bigint id PK
        bigint claim_id FK
        varchar author_type "CUSTOMER, EMPLOYEE"
        varchar author_name
        text body
        timestamp created_at
    }
```

## Design decisions 

| Decision | Reason |
|---|---|
| Claim status history with validity periods (started_at / ended_at) and a STATUS lookup table | No redundancy: the status lives in one place. Time spent in each status is calculated directly. Alternative considered: storing the current status in CLAIM (faster reads, but duplicated data). Trade-off accepted: getting the current status requires a join with the open history record. |
| Claim dates (created_at, resolved_at, closed_at) not stored in CLAIM | They are derived from the status history, so storing them would duplicate data. |
| `sla_due_at` stored, not calculated on each query | Depends on holidays from an external API; storing it makes reports simple and fast. |
| Priority stored in CLAIM, not in STATUS | Priority is a property of each claim and can be changed by a supervisor; two claims in the same status can have different priorities.|
| Catalog relations are 0..N (e.g. DEPARTMENT \|\|--o{ EMPLOYEE) | Mandatory membership is enforced by the NOT NULL FK on the child side; a parent (new department, category or service type) can exist before its first child. |
| assigned_employee_id nullable (EMPLOYEE \|o--o{ CLAIM) | A new claim (OPEN) has no agent until a supervisor assigns one. |

## Suggested indexes and constraints
- Unique partial index `claim_status_history(claim_id) WHERE ended_at IS NULL` → only one current status per claim.
- `CHECK (ended_at IS NULL OR ended_at > started_at)` → valid periods.
- Index `claim_status_history(status_id) WHERE ended_at IS NULL` → "claims currently in status X" (workload, open claims).
- Index `claim(sla_due_at)` → SLA breach report.
- Index `claim(customer_id)` → customer history.
