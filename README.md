# Ola Electric —  Sales, Complaints, and Market Share Analysis (2023) 

## Problem Statement
Ola Electric went from 50% EV market share in India to under 20% in less than 
a year. This project investigates what the data showed before the collapse.

## Tools Used
- **AWS S3** — Cloud storage for raw CSV data
- **AWS Athena** — Serverless SQL querying on S3
- **SQL** — Market share %, complaint-to-sales correlation
- **Power BI** — Interactive dashboard

## Data Sources
- VAHAN Portal (Government of India) — Monthly EV registrations by maker
- CCPA — Monthly complaint counts against Ola Electric

## Key Findings
- Ola dominated 2023 with 58% average market share vs competitors
- Registrations grew 66% from Jan to Dec 2023 (18,355 → 30,472 units)
- Complaints grew 8.5x in the same period — from 1,200 to 11,000/month
- Service infrastructure failed to scale with growth — early warning signal
  of the 2024 market share collapse from 50% to 19.6%

## SQL Queries
### Monthly registrations trend
```sql
SELECT month, month_num, registrations
FROM ev_registrations
WHERE maker = 'OLA ELECTRIC'
ORDER BY month_num;
```

### Market share % by maker
```sql
SELECT month, maker,
ROUND(registrations * 100.0 / 
SUM(registrations) OVER (PARTITION BY month_num), 1) AS market_share_pct
FROM ev_registrations
ORDER BY month_num, market_share_pct DESC;
```

### Complaints vs registrations
```sql
SELECT e.month, e.registrations, c.complaints
FROM ev_registrations e
JOIN ola_complaints c ON e.month_num = c.month_num
WHERE e.maker = 'OLA ELECTRIC'
ORDER BY e.month_num;
```

## Dashboard Preview
<img width="1302" height="732" alt="ola" src="https://github.com/user-attachments/assets/c536afa0-4f2f-4669-a8c2-200bde635f1f" />

