Here’s a **clear, step-by-step practical guide to using BigQuery** — exactly how you’d explain in an interview or follow in a real project.

***

# ✅ Step-by-Step Guide to Using BigQuery (Hands-on Flow)

***

# 🔹 Step 1: Create a GCP Project

1.  Go to: <https://console.cloud.google.com>
2.  Click **Project → New Project**
3.  Provide:
    *   Project name
    *   Organization (if needed)
4.  Click **Create**

***

# 🔹 Step 2: Enable BigQuery API

1.  In GCP Console:
    *   Go to **APIs & Services**
2.  Search: **BigQuery API**
3.  Click **Enable**

***

# 🔹 Step 3: Open BigQuery Console

1.  Navigate:
    👉 **BigQuery → SQL Workspace**

You’ll see:

*   Explorer panel (left)
*   Query editor (center)

***

# 🔹 Step 4: Create a Dataset (Like Schema)

👉 Dataset = container for tables

### Option 1 (UI):

1.  Click your project name
2.  Click **Create Dataset**
3.  Provide:
    *   Dataset ID (e.g., `sales_data`)
    *   Location (e.g., `US`)
4.  Click **Create**

***

# 🔹 Step 5: Create a Table

You have **3 common ways**:

***

## ✅ Method 1: Upload File (CSV, JSON)

1.  Click dataset → **Create Table**
2.  Source:
    *   Upload file (CSV/JSON)
3.  Destination:
    *   Table name: `customers`
4.  Schema:
    *   Auto-detect OR define manually
5.  Click **Create**

***

## ✅ Method 2: Using SQL

```sql
CREATE TABLE sales_data.customers (
  id INT64,
  name STRING,
  age INT64
);
```

***

## ✅ Method 3: Load from Cloud Storage

```sql
CREATE OR REPLACE TABLE sales_data.customers
LOAD DATA FROM FILES (
  format = 'CSV',
  uris = ['gs://bucket-name/file.csv']
);
```

***

# 🔹 Step 6: Query Data (Core Usage)

👉 This is the main purpose of BigQuery

### Example:

```sql
SELECT name, age
FROM sales_data.customers
WHERE age > 25;
```

👉 Click **Run**  
👉 Results appear instantly

***

# 🔹 Step 7: Use Aggregations

```sql
SELECT age, COUNT(*) AS total
FROM sales_data.customers
GROUP BY age;
```

***

# 🔹 Step 8: Use Joins

```sql
SELECT c.name, o.order_amount
FROM customers c
JOIN orders o
ON c.id = o.customer_id;
```

***

# 🔹 Step 9: Optimize using Partitioning

👉 While creating table:

```sql
CREATE TABLE sales_data.sales (
  id INT64,
  amount FLOAT64,
  sale_date DATE
)
PARTITION BY sale_date;
```

✅ Benefit:

*   Faster queries
*   Lower cost

***

# 🔹 Step 10: Use Clustering

```sql
CREATE TABLE sales_data.sales
PARTITION BY sale_date
CLUSTER BY customer_id;
```

✅ Improves performance for filtered queries

***

# 🔹 Step 11: Insert / Update Data

### Insert:

```sql
INSERT INTO sales_data.customers (id, name, age)
VALUES (1, 'John', 30);
```

***

### Update:

```sql
UPDATE sales_data.customers
SET age = 31
WHERE id = 1;
```

***

### Merge (Important for ETL):

```sql
MERGE target T
USING source S
ON T.id = S.id
WHEN MATCHED THEN
  UPDATE SET T.name = S.name
WHEN NOT MATCHED THEN
  INSERT (id, name) VALUES (S.id, S.name);
```

***

# 🔹 Step 12: Export Data

1.  Select table
2.  Click **Export**
3.  Choose:
    *   Cloud Storage
    *   CSV / JSON format

***

# 🔹 Step 13: Connect to Visualization Tools

You can connect:

*   Looker / Looker Studio
*   Tableau
*   Power BI

👉 Build dashboards directly

***

# 🔹 Step 14: Automate with Scheduled Queries

1.  Open query
2.  Click **Schedule**
3.  Define:
    *   Frequency (daily/hourly)
    *   Destination table

***

# 🔹 Step 15: Monitor Cost & Performance

👉 Important in real projects

*   Check:
    *   Query execution time
    *   Data scanned (in query UI)

✅ Tip:

> Always filter data to reduce cost

***

# ✅ Real Project Flow (End-to-End)

### Scenario: Sales Analytics

1.  Upload raw data → GCS
2.  Load into BigQuery
3.  Transform using SQL
4.  Store processed tables
5.  Create dashboards

***

# ✅ Best Practices (Interview MUST)

*   ✅ Use partitioning for large tables
*   ✅ Avoid SELECT \* (scan cost increases)
*   ✅ Prefer MERGE for updates
*   ✅ Use column filtering
*   ✅ Optimize joins

***

# ✅ Common Mistakes

❌ Running queries without filters  
❌ Frequent updates (use batch instead)  
❌ Not using partitioning  
❌ Large unoptimized joins

***

# ✅ Perfect Interview Summary

> “To use BigQuery, we first create a dataset and load data from sources like CSV or Cloud Storage. Then we query it using SQL, apply transformations, and optimize performance using partitioning and clustering. It’s mainly used for analytics, reporting, and building scalable data pipelines.”

***

# ✅ If You Want Next Level

I can help you with:
✅ Hands-on mini project (resume-ready)  
✅ BigQuery interview coding questions  
✅ Real-time ETL pipeline using BigQuery + Dataflow/Spark

Just tell me 👍
