# TIL: Databricks Learning Edition - Batch Ingestion with `COPY INTO`

## What I Learned

Today I learned that **Streaming Tables are not always the best choice for data ingestion in Databricks Learning Edition**.

For a static dataset such as the **Instacart CSV files**, a regular Delta table with an explicit schema and `COPY INTO` is a better approach for batch ingestion.

The goal is to keep the Bronze layer simple, predictable, and easy to understand.

## What I Initially Considered

I initially considered using Streaming Tables for the ingestion layer of the Instacart dataset.

However, for this use case, the source data is a collection of existing CSV files rather than a continuously arriving stream. Using streaming functionality adds unnecessary complexity when the requirement is simply to load a static dataset into the Bronze layer.

## Better Approach

For batch ingestion, I can:

1. Define the table schema explicitly.
2. Create the target table if it does not exist.
3. Use `COPY INTO` to load the CSV files.
4. Use a file pattern to make sure only the intended files are loaded.

Example:

```sql
-- ORDERS
CREATE TABLE IF NOT EXISTS `ftw-week-06`.`01-raw`.orders (
    order_id INT,
    user_id INT,
    eval_set STRING,
    order_number INT,
    order_dow INT,
    order_hour_of_day INT,
    days_since_prior_order DOUBLE
);

COPY INTO `ftw-week-06`.`01-raw`.orders
FROM '/Volumes/ftw-week-06/00-source/cloudflare/shared/week06/instacart_csv/'
FILEFORMAT = CSV
PATTERN = 'orders.*\.csv'
FORMAT_OPTIONS (
    'header' = 'true'
);
```

## Why This Is Better for This Workflow

Using `CREATE TABLE` with an explicit schema gives me more control over the expected data types.

For example:

* `order_id` → `INT`
* `user_id` → `INT`
* `eval_set` → `STRING`
* `order_number` → `INT`
* `order_dow` → `INT`
* `order_hour_of_day` → `INT`
* `days_since_prior_order` → `DOUBLE`

This is preferable to relying on automatic schema inference because I already know the expected structure of the source data.

The `PATTERN` option is also useful because if a new CSV file arrives, there would be multiple orders.csv files in the source Volume. Instead of loading everything from the directory, I can specify:

```sql
PATTERN = 'orders.*\.csv'
```

This matches the orders file and prevents unrelated CSV files from being ingested into the `orders` table.

## Key Takeaway

**Not every ingestion problem needs streaming.**

For a static batch dataset in Databricks Learning Edition, a straightforward pattern is:

```text
CSV Files
   ↓
COPY INTO
   ↓
Bronze / Raw Delta Table
   ↓
Data Quality & Transformation
   ↓
Silver
   ↓
Gold
```

The important lesson for me is to choose the ingestion method based on the characteristics of the source data.

For static files, **batch ingestion with `COPY INTO` is simpler and more appropriate than introducing streaming tables unnecessarily**.

> I learned that using a more advanced Databricks feature does not automatically make a pipeline better. For static batch data, simple `CREATE TABLE` + `COPY INTO` ingestion can be the cleaner engineering choice.
