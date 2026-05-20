Great — this combination is exactly what interviewers expect from a **Data Engineer**. I’ll give you all 3 in a **practical, resume-ready + interview-focused format**.

***

# ✅ 1. Hands-on Mini Project (Resume Ready)

## 🎯 Project: E-Commerce Sales Analytics using BigQuery

***

## ✅ Problem Statement

Build a pipeline to:

*   Ingest sales data
*   Transform it
*   Perform analytics
*   Create insights

***

## ✅ Tech Stack

*   GCS (storage)
*   BigQuery (analytics)
*   SQL (transformations)
*   Optional: Dataflow/Spark

***

## ✅ Step-by-Step Implementation

### 🔹 Step 1: Prepare Dataset

Create sample CSV:

    order_id,customer_id,product,amount,order_date
    1,101,Mobile,20000,2024-01-01
    2,102,Laptop,50000,2024-01-02
    3,101,Headphones,2000,2024-01-03

***

### 🔹 Step 2: Upload to Google Cloud Storage

*   Create bucket
*   Upload file

***

### 🔹 Step 3: Load into BigQuery

```sql
CREATE OR REPLACE TABLE sales.raw_orders
(
  order_id INT64,
  customer_id INT64,
  product STRING,
  amount FLOAT64,
  order_date DATE
);
```

***

### 🔹 Step 4: Transform Data

```sql
CREATE OR REPLACE TABLE sales.cleaned_orders AS
SELECT 
  order_id,
  customer_id,
  product,
  amount,
  order_date,
  EXTRACT(YEAR FROM order_date) AS year
FROM sales.raw_orders
WHERE amount > 0;
```

***

### 🔹 Step 5: Analytics Queries

#### ✅ Total Revenue

```sql
SELECT SUM(amount) AS total_revenue
FROM sales.cleaned_orders;
```

#### ✅ Revenue by Product

```sql
SELECT product, SUM(amount) revenue
FROM sales.cleaned_orders
GROUP BY product
ORDER BY revenue DESC;
```

#### ✅ Top Customers

```sql
SELECT customer_id, SUM(amount) total_spent
FROM sales.cleaned_orders
GROUP BY customer_id
ORDER BY total_spent DESC;
```

***

### 🔹 Step 6: Optimization

```sql
CREATE TABLE sales.optimized_orders
PARTITION BY order_date
CLUSTER BY customer_id AS
SELECT * FROM sales.cleaned_orders;
```

***

## ✅ Resume Points (IMPORTANT)

👉 Add this:

*   Designed and implemented a **BigQuery-based data warehouse** for e-commerce analytics
*   Built ETL pipeline using **GCS → BigQuery**
*   Optimized queries using **partitioning and clustering**
*   Developed SQL queries for **revenue analysis and customer insights**
*   Reduced query cost by **filtering and column-level optimization**

***

# ✅ 2. BigQuery Interview Coding Questions

***

## ✅ Q1: Find Duplicate Records

```sql
SELECT order_id, COUNT(*)
FROM sales.orders
GROUP BY order_id
HAVING COUNT(*) > 1;
```

***

## ✅ Q2: Second Highest Salary

```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```

***

## ✅ Q3: Running Total

```sql
SELECT 
  order_date,
  SUM(amount) OVER (ORDER BY order_date) AS running_total
FROM sales.orders;
```

***

## ✅ Q4: Top 3 Products per Category

```sql
SELECT *
FROM (
  SELECT product, category, revenue,
         RANK() OVER (PARTITION BY category ORDER BY revenue DESC) rnk
  FROM products
)
WHERE rnk <= 3;
```

***

## ✅ Q5: Latest Record per Customer

```sql
SELECT *
FROM (
  SELECT *,
         ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) rn
  FROM orders
)
WHERE rn = 1;
```

***

## ✅ Q6: Pivot Example

```sql
SELECT *
FROM (
  SELECT product, amount
  FROM sales.orders
)
PIVOT (
  SUM(amount) FOR product IN ('Mobile', 'Laptop')
);
```

***

## ✅ Q7: MERGE (Upsert)

```sql
MERGE target T
USING source S
ON T.id = S.id
WHEN MATCHED THEN
  UPDATE SET T.value = S.value
WHEN NOT MATCHED THEN
  INSERT (id, value) VALUES (S.id, S.value);
```

***

# ✅ 3. Real-Time ETL Pipeline (BigQuery + Dataflow/Spark)

***

## 🎯 Scenario: Real-Time Clickstream Processing

***

## ✅ Architecture

    User Events → Pub/Sub → Dataflow / Spark Streaming → BigQuery → Dashboard

***

## ✅ Step-by-Step Flow

***

### 🔹 Step 1: Event Generation

*   Website sends events (clicks, views)

Example:

```json
{
  "user_id": 101,
  "event": "click",
  "timestamp": "2026-01-01T10:00:00"
}
```

***

### 🔹 Step 2: Ingestion (Pub/Sub)

*   Events pushed to topic

***

### 🔹 Step 3: Processing (Dataflow / Spark)

#### ✅ Transformations:

*   Validate data
*   Remove duplicates
*   Add derived columns

***

### Example (Spark Streaming)

```python
df = spark.readStream.format("kafka").load()

cleaned = df.filter("user_id IS NOT NULL") \
            .withColumn("event_date", to_date("timestamp"))
```

***

### 🔹 Step 4: Load into BigQuery

```python
cleaned.writeStream \
  .format("bigquery") \
  .option("table", "dataset.events") \
  .start()
```

***

### 🔹 Step 5: Analytics (BigQuery)

```sql
SELECT event, COUNT(*) 
FROM dataset.events
GROUP BY event;
```

***

### 🔹 Step 6: Dashboard

*   Connect Looker / Power BI
*   Real-time metrics

***

## ✅ Why This Architecture is Used

| Need                | Solution         |
| ------------------- | ---------------- |
| Real-time ingestion | Pub/Sub          |
| Data processing     | Spark / Dataflow |
| Analytics           | BigQuery         |
| Visualization       | Looker           |

***

## ✅ Interview Explanation (Perfect Answer)

> “In a real-time ETL pipeline, data is ingested via Pub/Sub, processed using Dataflow or Spark Streaming for cleaning and enrichment, and then loaded into BigQuery for analytics. BigQuery is used as the serving layer for BI dashboards.”

***

# ✅ Final Key Takeaways

*   ✅ BigQuery = Analytics (SQL engine using Dremel)
*   ✅ Spark/Dataflow = Processing layer
*   ✅ Use both together in real-world projects
*   ✅ Prefer batch + streaming pipelines

***
