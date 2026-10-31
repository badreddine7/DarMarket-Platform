# 02 - Requirements

## Stakeholder interview log (simulated)
Write each interview as Q and A. Replace the placeholders with your own reasoning as you "interview".

| Stakeholder | Question | Answer (simulated) |
|---|---|---|
| Finance | How do you define revenue? | Delivered orders only, net of cancellations and refunds, in MAD |
| Finance | When is a day closed? | Midnight Africa/Casablanca; late status changes up to 7 days must be absorbed |
| Logistics | What is "late"? | Delivered after the promised date, excluding customer-requested reschedules |
| Marketing | What is an "active customer"? | At least one delivered order in the last 90 days |
| Sales | How much history? | 24 months for trends |
| DPO | Which fields are personal data? | Name, email, phone, address |

## Functional requirements
| ID | Requirement | Priority |
|---|---|---|
| F1 | Ingest orders, order items, customers, products, payments and shipments incrementally from Postgres | Must |
| F2 | Ingest daily carrier delivery files (CSV/JSON) from a landing folder | Must |
| F3 | Ingest daily weather per delivery city from an external API | Should |
| F4 | Publish a star schema with documented grain and business definitions | Must |
| F5 | Track history of product price and customer city changes (SCD type 2) | Should |
| F6 | Quarantine invalid records with a reason; never drop silently | Must |
| F7 | Support backfilling any date range without duplicates | Must |
| F8 | Serve dashboards for the five business questions | Must |

## Non-functional requirements
| ID | Category | Requirement |
|---|---|---|
| N1 | Freshness | Gold tables updated by 07:00 Africa/Casablanca daily |
| N2 | Accuracy | Daily revenue reconciles with the source within 0.1%; reconciliation report published each run |
| N3 | Reliability | Idempotent tasks; retries with backoff; failures alert within 10 minutes |
| N4 | Scalability | Handles 10x current volume (10,000 orders/day) without redesign |
| N5 | Security | PII hashed or masked outside the restricted bronze zone; least-privilege roles; no secrets in Git |
| N6 | Privacy | Retention: raw PII deleted after 24 months; deletion requests honored within 30 days |
| N7 | Cost | Runs on free tiers or a laptop; cloud cost estimate documented for production |
| N8 | Maintainability | All changes via pull request with CI; every model documented and tested |
| N9 | Observability | Every run logs rows in, rows out, rejects, duration; dashboard of pipeline health |
| N10 | Recoverability | Any day can be rebuilt from bronze; runbook documents how |

## Assumptions to validate
- Source Postgres has a reliable `updated_at` column on every table (to be proven in Phase 1; if not, design CDC).
- Carriers send files by 03:00; some arrive late or malformed.
- Timezone is stored as UTC in the source and converted to Africa/Casablanca in gold.

## Open questions (resolve before Phase 2 ends)
- How are refunds recorded: as new rows, or as status updates?
- Do cash-on-delivery orders count as revenue before collection?
