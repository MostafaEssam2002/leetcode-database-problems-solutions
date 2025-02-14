# Monthly Transactions Report

## Problem Description

### Table: Transactions

| Column Name   | Type    |
|--------------|---------|
| id           | int     |
| country      | varchar |
| state        | enum    |
| amount       | int     |
| trans_date   | date    |

- `id` is the **primary key** (column with unique values) for this table.
- This table contains **information about incoming transactions**.
- The `state` column is an **enum** with values **["approved", "declined"]**.

---

### Task:
Write a query to **find for each month and country**:
- The **number of transactions**.
- Their **total amount**.
- The **number of approved transactions**.
- The **total amount of approved transactions**.

Return the result table **in any order**.

---

## Example:

### Example 1:

#### Input:
**Transactions table:**

| id   | country | state    | amount | trans_date |
|------|---------|----------|--------|------------|
| 121  | US      | approved | 1000   | 2018-12-18 |
| 122  | US      | declined | 2000   | 2018-12-19 |
| 123  | US      | approved | 2000   | 2019-01-01 |
| 124  | DE      | approved | 2000   | 2019-01-07 |

---

#### Output:

| month   | country | trans_count | approved_count | trans_total_amount | approved_total_amount |
|---------|---------|-------------|----------------|--------------------|-----------------------|
| 2018-12 | US      | 2           | 1              | 3000               | 1000                  |
| 2019-01 | US      | 1           | 1              | 2000               | 2000                  |
| 2019-01 | DE      | 1           | 1              | 2000               | 2000                  |

---

#### Explanation:
- **For December 2018 in the US**:
  - There are **2 transactions** with a **total amount of $3000**.
  - **1 transaction was approved**, with a **total approved amount of $1000**.
- **For January 2019 in the US**:
  - **1 transaction** with a **total amount of $2000**.
  - **1 approved transaction** with a **total approved amount of $2000**.
- **For January 2019 in Germany (DE)**:
  - **1 transaction** with a **total amount of $2000**.
  - **1 approved transaction** with a **total approved amount of $2000**.

---

## Solution:

```sql
SELECT 
    DATE_FORMAT(trans_date, '%Y-%m') AS month, 
    country, 
    COUNT(transactions.id) AS trans_count,
    COUNT(CASE WHEN state = 'approved' THEN state END) AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM transactions 
GROUP BY month, country;
