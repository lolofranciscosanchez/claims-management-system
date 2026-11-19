# Entity-Relationship Diagram (draft)

> Draft logical model. Review it, adjust it to your own design decisions and then implement it as a Flyway migration (`src/main/resources/db/migration/V1__create_schema.sql`).

```mermaid
erDiagram
    CUSTOMER ||--o{ CUSTOMER_SERVICE : "has"
    SERVICE_TYPE ||--o{ CUSTOMER_SERVICE : "is of type"
    CUSTOMER ||--o{ CLAIM : "opens"
    CUSTOMER_SERVICE ||--o{ CLAIM : "affected by"
    CLAIM_CATEGORY ||--o{ CLAIM : "classifies"
    DEPARTMENT ||---{ CLAIM_CATEGORY : "is responsible for"
    DEPARTMENT ||--o{ EMPLOYEE : "employs"
    EMPLOYEE ||--o{ CLAIM : "is assigned"
    CLAIM ||--o{ CLAIM_STATUS_HISTORY : "records"
    CLAIM ||--o{ CLAIM_COMMENT : "has"

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
        varchar status
        varchar priority
        text description
        timestamp created_at
        timestamp sla_due_at
        timestamp resolved_at "nullable"
        timestamp closed_at "nullable"
        smallint rating "nullable, 1-5"
    }
    CLAIM_STATUS_HISTORY {
        bigint id PK
        bigint claim_id FK
        varchar from_status "nullable"
        varchar to_status
        varchar changed_by
        timestamp changed_at
        varchar note
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

## Design decisions (to complete)

| Decision | Reason |
|---|---|
| Status and priority stored as `VARCHAR` + `CHECK` constraint instead of lookup tables | Fixed set of values controlled by the application's state machine. |
| `claim_status_history` as a separate table | Full audit trail; enables resolution-time reports. |
| `sla_due_at` stored, not calculated on each query | Depends on holidays from an external API; storing it makes reports simple and fast. |
| *Add your own...* | |

## Suggested indexes
- `claim(status, sla_due_at)` → workload queries and SLA breach report.
- `claim(customer_id)` → customer history.
- `claim_status_history(claim_id, changed_at)` → timeline of a claim.
