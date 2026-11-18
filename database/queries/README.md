# SQL Queries

Analytical and operational queries written in plain PostgreSQL. They are run against the seeded database and later reused by the reporting endpoints.

| File | Business question | SQL concepts |
|---|---|---|
| `01_claims_by_status.sql` | How many claims are in each status? | `GROUP BY`, `COUNT` |
| `02_avg_resolution_time_by_department.sql` | What is the average resolution time (hours) per department? | `JOIN`, `AVG`, `EXTRACT(EPOCH ...)` |
| `03_top_categories.sql` | Which are the top 5 claim categories this month? | `GROUP BY`, `ORDER BY`, `LIMIT`, date filters |
| `04_sla_breaches.sql` | Which unresolved claims have passed their SLA due date? | `WHERE`, `NOW()`, multiple `JOIN` |
| `05_agent_workload.sql` | How many active claims does each agent have? | `LEFT JOIN`, `COALESCE` |
| `06_customers_with_repeated_claims.sql` | Which customers opened more than 3 claims in 30 days? | `HAVING`, subquery |
| `07_monthly_trend.sql` | Claims opened per month and % resolved within SLA. | `DATE_TRUNC`, `CASE WHEN`, `FILTER` |
| `08_agent_ranking.sql` | Rank agents by claims resolved per department. | Window functions (`RANK() OVER (PARTITION BY ...)`) |
| `09_claim_timeline.sql` | Full status timeline of one claim with time spent in each status. | `LAG()` window function |
| `10_view_open_claims.sql` | View with open claims enriched with customer, category and agent. | `CREATE VIEW` |

> Write each query with a header comment explaining the business question, and paste a sample of the result below it.
