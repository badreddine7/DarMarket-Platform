# 04 - Roadmap (8 weeks, part-time)

Each week ends with a **deliverable you can show**. Step numbers refer to the 22-step lifecycle.

| Week | Phase | Steps | What you do | Deliverable (definition of done) |
|---|---|---|---|---|
| 1 | Understand | 1-3 | Problem statement, requirements, source inventory, interview log | Docs 01-03 reviewed and open questions listed |
| 2 | Design | 4-6 | Architecture diagram, star schema, data contracts, first ADRs | Diagram, ERD, data dictionary, 3+ ADRs |
| 3 | Environments | 7-9 | Docker Compose with dev and staging, repo standards, secrets, DB roles, build the **source system generator** | `docker compose up` gives a Postgres source filling with daily data |
| 4 | Build (slice) | 10-11 | Thin end-to-end slice: orders only, incremental extract to bronze; then carriers and weather | Orders flow source -> bronze, reruns create no duplicates |
| 5 | Build (model) | 12 | dbt silver and gold, PII hashing, SCD2 for products, quarantine | Star schema live, revenue reconciles with source |
| 6 | Orchestrate | 13-14 | Airflow DAG, dbt tests, source freshness, alerting | Failing check fails the run and sends an alert |
| 7 | Test and release | 15-17 | Unit and integration tests, CI/CD, reconciliation report, load test at 10x, simulated UAT | Green CI, load test report, UAT sign-off note |
| 8 | Operate | 18-22 | Deploy, backfill drill, monitoring dashboard, runbooks, handover, retrospective | Runbooks written, backfill proven, retrospective doc |

## Rules for yourself
- Do not start Week N+1 until Week N's deliverable exists in the repo.
- Every significant choice gets an ADR in `docs/adr/` (context, options, decision, consequences).
- Every pipeline change goes through a pull request, even if you are the only developer.
- Keep a `CHANGELOG.md` and a decision log. Interviewers love specifics ("I changed X because Y broke").

## Stretch goals (after Week 8)
- Deploy to a cloud free tier with Terraform.
- Add a streaming path for order events (Kafka or Redpanda).
- Add column-level lineage and a data catalog entry per table.
- Add cost and performance benchmarks to the README.
