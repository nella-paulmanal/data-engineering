# TIR: Quantifying Data Quality Across a Multi-Table Dataset

## Context

I recently watched a YouTube video about data quality in Databricks and learned about **Expectations** in Lakeflow Declarative Pipelines.

One thing that confused me was learning that Expectations are associated with the Advanced edition. Since I am working with **Databricks Free Edition**, my first question was:

> **If I don't have Expectations, how am I supposed to implement data quality?**

My initial assumption was that I would need the Advanced edition to properly perform data quality checks.

After looking into alternatives, I realized that I can still implement data quality validation using **SQL queries, validation tables, and Delta tables**. The limitation is mainly that I don't have the built-in Expectations feature for automatically enforcing and monitoring those rules.

So my plan is to build a simple SQL-based data quality framework instead.

---

## The First Problem: Where Do the DQ Results Go?

I initially thought about creating a table like:

| Table | Column | Completeness | Uniqueness | Validity | Referential Integrity | Business Rule | Volume | Status | Failure Count |
| ----- | ------ | ------------ | ---------- | -------- | --------------------- | ------------- | ------ | ------ | ------------: |

This works for documenting individual checks, but I realized that it creates another question:

> **How do I turn all these individual column-level checks into an overall picture of data quality?**

For example, `orders.order_id` can have a completeness score of 100%, while `products.aisle_id` might have a completeness score of 99.998%.

These are useful measurements, but they only tell me about individual columns.

They do not directly tell me:

> **How healthy is the entire dataset?**

---

## The Key Realization

I realized that data quality needs to be viewed at multiple levels.

### Level 1: Individual Check

A check is performed on a specific column in a specific table.

```text
orders.order_id
        ↓
Completeness
        ↓
100%
```

### Level 2: Table Health

The individual checks can be aggregated to describe the quality of an entire table.

```text
orders
   ↓
Completeness
Uniqueness
Validity
Referential Integrity
Business Rules
   ↓
Table Quality Score
```

### Level 3: Dataset Health

Since my dataset contains multiple related tables, the table-level measurements can then be aggregated into a dataset-level view.

```text
orders
products
aisles
departments
      ↓
DQ Validation
      ↓
Table-level metrics
      ↓
Dimension-level metrics
      ↓
Overall Dataset Health
```

This helped me understand that I should not jump directly from:

```text
column check → dataset score
```

Instead, I need an aggregation layer.

---

## My Planned DQ Structure

I want my validation results to capture the individual checks first.

For example:

| Table    | Column                 | Dimension    | Total Records | Failure Count |   Score | Status  |
| -------- | ---------------------- | ------------ | ------------: | ------------: | ------: | ------- |
| orders   | order_id               | Completeness |     3,421,083 |             0 |    100% | PASS    |
| orders   | order_id               | Uniqueness   |     3,421,083 |             0 |    100% | PASS    |
| orders   | user_id                | Completeness |     3,421,083 |             0 |    100% | PASS    |
| orders   | days_since_prior_order | Completeness |     3,421,083 |       206,209 |  93.97% | WARNING |
| products | aisle_id               | Completeness |        49,688 |             1 | 99.998% | WARNING |

This table becomes my **detailed DQ results table**.

The `failure_count` allows me to quantify the problem, while `score` allows me to compare quality across checks.

---

## Then Aggregate by Quality Dimension

Instead of putting every column-level result directly onto the dashboard, I can aggregate the results by quality dimension.

For example:

```text
Completeness              99.82%
Uniqueness                99.97%
Validity                  99.94%
Referential Integrity     99.91%
Business Rules            99.88%
```

This gives me a **quality profile for the dataset**.

The dashboard can then show the major dimensions instead of hundreds of individual checks.

---

## The Dataset-Level Question

The biggest question I still need to answer carefully is:

> **How should I calculate one overall data quality score when the dataset contains multiple tables of different sizes?**

I realized that simply averaging all column scores may be misleading.

For example:

```text
orders       = 3,421,083 rows
products     =    49,688 rows
```

If I calculate:

```text
(orders quality + products quality) / 2
```

I am treating both tables as equally important, even though one contains far more records.

Therefore, I need to consider the number of records or values being evaluated when calculating dimension-level scores.

For example, completeness can be calculated as:

```text
Completeness =
(total expected values - missing values)
----------------------------------------
total expected values
× 100
```

This allows the dataset-level completeness score to reflect the actual amount of data being evaluated.

---

## PASS, WARNING, and FAIL

Another question I had was:

> **How do I quantify PASS, WARNING, and FAIL across the whole dataset?**

The answer is to count the validation results.

For example:

```text
PASS       42 checks
WARNING     5 checks
FAIL        2 checks
```

These counts can become dashboard KPIs.

However, I realized that the number of checks is different from the amount of data affected.

For example:

```text
1 failed check
```

could represent:

```text
1 affected record
```

or:

```text
200,000 affected records
```

Therefore, I want the dashboard to show both:

* Number of PASS/WARNING/FAIL checks
* Failure counts / affected records
* Quality percentages

This prevents a single failed check from being interpreted as automatically catastrophic.

---

## Planned Dashboard

My current plan is to build a Data Quality Dashboard with several levels of information.

### Overall Dataset Health

```text
Overall Data Quality Score
          99.76%
```

This will be a calculated KPI based on the defined quality dimensions and documented aggregation method.

### Quality Dimensions

```text
Completeness              99.82%
Uniqueness                99.97%
Validity                  99.94%
Referential Integrity     99.91%
Business Rules            99.88%
```

### Validation Status

```text
PASS       42
WARNING     5
FAIL        2
```

### Problem Areas

The dashboard should also identify which tables, columns, and dimensions are responsible for the warnings and failures.

For example:

| Table    | Column                 | Dimension    |   Score | Status  | Failure Count |
| -------- | ---------------------- | ------------ | ------: | ------- | ------------: |
| orders   | days_since_prior_order | Completeness |  93.97% | WARNING |       206,209 |
| products | aisle_id               | Completeness | 99.998% | WARNING |             1 |

This allows the dashboard to answer not only:

> "How good is the dataset?"

but also:

> "Where are the problems?"

---

One thing I want to document clearly is that an **overall data quality score is not a universal metric**.

There is no single magical formula that says:

> "This dataset is exactly 99.76% good."

The score depends on:

* Which quality dimensions I choose
* Which columns are subject to each check
* How I calculate each dimension
* How I handle tables of different sizes
* Whether some dimensions receive more weight than others
* What thresholds define PASS, WARNING, and FAIL

Therefore, if I create an overall score, I need to **document the methodology behind it**.

The score should be treated as a monitoring KPI, not as an absolute scientific measurement of data quality.

---

## My Current Plan

For my Databricks Free Edition project, I plan to implement:

```text
Bronze
  ↓
Silver
  ↓
SQL Data Quality Checks
  ↓
DQ Validation Results Table
  ↓
Dimension-Level Aggregation
  ↓
Dataset-Level Metrics
  ↓
Data Quality Dashboard
```

The DQ validation table will be the source of truth for the dashboard.

I will use SQL and `UNION ALL` to consolidate individual validation results.

The dashboard will then quantify:

1. PASS / WARNING / FAIL checks
2. Failure counts
3. Completeness %
4. Uniqueness %
5. Validity %
6. Referential integrity %
7. Business rule compliance %
8. Overall dataset health
9. Problematic tables and columns

## What I Learned

My biggest realization was that **data quality is hierarchical**.

A column-level check tells me about one attribute.

A collection of checks tells me about a table.

Aggregated checks across related tables tell me about the dataset.

So the problem I am trying to solve is not simply:

> "How do I run data quality checks?"

It is:

> **"How do I turn many individual data quality measurements into a meaningful, explainable view of the health of an entire multi-table dataset?"**

That is the part I want to explore and implement next.
