# SQL Project Plan: Loan Portfolio Analysis

**Why this project:** SQL is the first skill on your profile but there is no SQL project in your featured repos. Most analyst interviews include a SQL test, so this closes your biggest gap. It also reuses `loan_data.csv`, which is already in your Loan Default repo, so you don't need a new dataset to start.

**Suggested repo name:** `loan-portfolio-sql-analysis`
**Database:** SQLite (free, no setup, works in "DB Browser for SQLite") or PostgreSQL. Either is fine.

## Repo structure

```
loan-portfolio-sql-analysis/
├── data/loan_data.csv
├── sql/
│   ├── 01_create_and_clean.sql
│   ├── 02_portfolio_kpis.sql
│   ├── 03_trends_and_windows.sql
│   └── 04_segmentation.sql
├── results/            # screenshots or CSV exports of key query results
└── README.md           # business questions, findings, recommendations
```

## Business questions (put these in the README)

1. What is the overall default rate, and how has it changed month to month?
2. Do individual or business borrowers default more?
3. Does loan size relate to default?
4. Which months had the biggest lending volume, and how does each month compare to the previous one?
5. What share of total lending does each customer segment hold?

## Starter queries

Your dataset has: `loan_id`, `loan_date`, `customer_type`, `loan_amount`, `loan_status` (Active / Paid / Default). Adjust column names to match your file.

**1. Cleaning and checks**
```sql
-- Duplicates
SELECT loan_id, COUNT(*) FROM loans GROUP BY loan_id HAVING COUNT(*) > 1;

-- Missing or invalid values
SELECT COUNT(*) FROM loans WHERE loan_amount IS NULL OR loan_amount <= 0;
```

**2. Portfolio KPIs**
```sql
SELECT
  COUNT(*)                                             AS total_loans,
  SUM(loan_amount)                                     AS total_disbursed,
  ROUND(AVG(loan_amount), 0)                           AS avg_loan,
  ROUND(100.0 * SUM(CASE WHEN loan_status = 'Default' THEN 1 ELSE 0 END) / COUNT(*), 2) AS default_rate_pct,
  ROUND(100.0 * SUM(CASE WHEN loan_status = 'Default' THEN loan_amount ELSE 0 END) / SUM(loan_amount), 2) AS default_value_pct
FROM loans;
```

**3. Default rate by customer type (CASE + GROUP BY)**
```sql
SELECT customer_type,
       COUNT(*) AS loans,
       ROUND(100.0 * SUM(loan_status = 'Default') / COUNT(*), 2) AS default_rate_pct
FROM loans
GROUP BY customer_type;
```
(`SUM(loan_status = 'Default')` works in SQLite/MySQL. In PostgreSQL use `SUM(CASE WHEN loan_status='Default' THEN 1 ELSE 0 END)`.)

**4. Month-over-month lending with a window function**
```sql
WITH monthly AS (
  SELECT strftime('%Y-%m', loan_date) AS month,   -- PostgreSQL: TO_CHAR(loan_date,'YYYY-MM')
         SUM(loan_amount) AS disbursed
  FROM loans
  GROUP BY 1
)
SELECT month,
       disbursed,
       LAG(disbursed) OVER (ORDER BY month) AS prev_month,
       ROUND(100.0 * (disbursed - LAG(disbursed) OVER (ORDER BY month))
             / LAG(disbursed) OVER (ORDER BY month), 1) AS mom_growth_pct
FROM monthly;
```

**5. Segment share and ranking**
```sql
WITH seg AS (
  SELECT CASE WHEN loan_amount > 100000 THEN 'High'
              WHEN loan_amount > 50000  THEN 'Medium'
              ELSE 'Low' END AS segment,
         SUM(loan_amount) AS amount
  FROM loans GROUP BY 1
)
SELECT segment, amount,
       ROUND(100.0 * amount / SUM(amount) OVER (), 1) AS pct_of_portfolio,
       RANK() OVER (ORDER BY amount DESC) AS rnk
FROM seg;
```

## An honest limitation to fix

With only five columns you can't calculate **PAR30** (Portfolio at Risk, the standard microfinance metric: value of loans more than 30 days overdue ÷ total outstanding). To do it properly, either:

- add columns such as `due_date`, `amount_repaid`, `days_past_due` (if you generate synthetic data, **say clearly in the README that it's synthetic**), or
- find a public lending dataset that includes repayment dates.

Adding PAR30 and a repayment-cohort query (loans grouped by disbursement month, tracking how many defaulted) would make this project stand out for Kenyan banks, SACCOs and fintechs.

## README checklist

- [ ] One-paragraph business problem
- [ ] The 5 questions above, each with the query and a 1–2 line answer with real numbers
- [ ] Screenshot of results
- [ ] Recommendation section ("Business customers default X% more, so consider tighter limits or collateral")
- [ ] Skills demonstrated: joins/aggregation, CASE, CTEs, window functions (LAG, RANK, SUM OVER)
- [ ] Note on whether data is real or synthetic

## Bonus

If you want a second SQL dataset, your `E-commerce-Sales-Dataset` repo could feed a join-heavy project (orders, customers, products) to show multi-table joins.
