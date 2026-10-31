# DarMarket Analytics Platform

A production-style analytics platform for a fictional Moroccan online marketplace (DarMarket, Casablanca).
The goal of this repo is to practice the **full data engineering lifecycle**, not just the code:
requirements, design, environments, build, test, release, operate.

Status: **Phase 1 - Understand the problem** (see `docs/04_roadmap.md`)

## Docs (read in this order)
1. `docs/01_problem_statement.md` - who needs what, and how we measure success
2. `docs/02_requirements.md` - functional and non-functional requirements
3. `docs/03_source_inventory.md` - every source: owner, format, risks
4. `docs/04_roadmap.md` - 8-week plan mapped to the 22 lifecycle steps
5. `docs/adr/` - architecture decision records (one per major choice)
6. `docs/runbooks/` - operating procedures (added in Phase 6)

## Planned stack
Postgres (source system) -> Python incremental extract -> Parquet bronze -> dbt + DuckDB (silver/gold)
-> Metabase dashboards. Airflow orchestrates, Docker Compose provides environments, GitHub Actions
runs CI/CD. Stack choices are confirmed or changed in Phase 2 via ADRs.
