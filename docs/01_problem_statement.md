# 01 - Problem Statement
Change 18

## Context
DarMarket is a fictional online marketplace in Morocco (about 50,000 customers, roughly 1,000 orders per
day, six carriers, payments in MAD by card and cash on delivery). Data lives in an operational Postgres
database, carrier delivery files and a weather API. Reporting is done by hand in spreadsheets, takes
about two days to assemble, and finance and sales numbers regularly disagree.

## Problem
There is no trusted, automated, daily view of sales, delivery performance and customer behavior.
Decisions on carriers, promotions and stock are made on stale or conflicting numbers.

## Goal
Deliver an automated analytics platform that publishes trusted daily metrics by 07:00 and lets each
team answer its own questions without asking engineering.

## Stakeholders (simulated; interview them in writing, see 02_requirements.md)
| Role | Needs | Decision it supports |
|---|---|---|
| Head of Sales | Daily revenue by city and category, top products | Promotions, assortment |
| Logistics Manager | On-time delivery rate by carrier and city, delay causes | Carrier contracts |
| Finance Controller | Revenue that reconciles with the source of truth | Month-end close |
| Marketing Lead | Retention cohorts, repeat purchase rate | Campaigns |
| Data Protection Officer | No unprotected personal data outside restricted zones | Compliance (Law 09-08) |

## Business questions the platform must answer
1. What was yesterday's revenue by city and category, and how does it compare with last week?
2. Which carriers deliver late, in which cities, and does weather explain it?
3. What share of customers order again within 30 and 90 days?
4. Which products are top sellers and which are at risk of stock-out?
5. What is the payment failure rate, by method?

## Success metrics
| Metric | Target |
|---|---|
| Gold tables ready | by 07:00 every day |
| Revenue reconciliation vs source | within 0.1% |
| Pipeline run success rate | 99% over 30 days, with failures alerting within 10 minutes |
| Rows rejected by validation | under 1%, all visible in a quarantine table |
| Time to answer a new KPI question | under 1 day, without a new pipeline |

## Out of scope (v1)
Real-time dashboards, ML models, customer-facing analytics, data from marketing platforms.
