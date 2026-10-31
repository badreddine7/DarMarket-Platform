# 03 - Source Inventory

| # | Source | Owner (simulated) | Type / access | Update pattern | Volume | Key fields | Known risks |
|---|---|---|---|---|---|---|---|
| S1 | `darmarket_oltp` Postgres: customers, products, orders, order_items, payments, shipments | Backend team | Postgres, read-only replica user | Continuous inserts and status updates | ~1k orders/day, ~50k customers | `order_id`, `updated_at` | Hard deletes?; `updated_at` not always maintained; schema changes without notice |
| S2 | Carrier delivery files | 6 carriers (logistics) | CSV/JSON dropped in landing folder (SFTP-style) | Daily by ~03:00, sometimes late | ~1k rows/day total | `tracking_id`, `delivered_at` | Different formats per carrier; duplicates; timezone ambiguity; late corrections |
| S3 | Weather API (Open-Meteo) | External, no SLA | HTTPS JSON, no API key | Hourly forecast and history | 8 cities x 24h | `city`, `time` | Rate limits, outages, schema drift; no SLA |
| S4 | City reference data | Data team | Static CSV in repo | Rarely | ~30 rows | `city_id` | Naming variants (Fes / Fès / Fez) |

## Per-source checklist (complete before Phase 2)
For each source answer: How do we authenticate? What is the primary key? How do we detect changes?
What does a delete look like? What is the timezone? What is the expected arrival time? Who do we call
when it breaks? Which fields are PII?

## Risk register (starter)
| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Source schema change breaks ingestion | High | High | Schema contract + drift detection; fail loudly in staging first |
| Late carrier files | High | Medium | Rolling 7-day reprocessing window |
| API outage | Medium | Low | Retries; weather is optional for gold KPIs |
| PII leak into analytics layer | Low | Very high | Hashing in silver; access roles; automated PII scan test |
| `updated_at` unreliable | Medium | High | Validate in Phase 1; fall back to CDC or full-snapshot diff |
