# Query Description and Solution

## Table Definitions

### Visits Table

| Column Name | Type    |
|-------------|---------|
| visit_id    | int     |
| customer_id | int     |

- **visit_id**: Unique identifier for each visit.
- This table contains information about the customers who visited the mall.

### Transactions Table

| Column Name    | Type    |
|----------------|---------|
| transaction_id | int     |
| visit_id       | int     |
| amount         | int     |

- **transaction_id**: Unique identifier for each transaction.
- This table contains information about transactions made during the visit.

---

## Problem Statement

Find the IDs of users who visited the mall without making any transactions and the number of times they made such visits. Return the result table sorted in any order.

---

## Example Input and Output

### Input

**Visits Table**

| visit_id | customer_id |
|----------|-------------|
| 1        | 23          |
| 2        | 9           |
| 4        | 30          |
| 5        | 54          |
| 6        | 96          |
| 7        | 54          |
| 8        | 54          |

**Transactions Table**

| transaction_id | visit_id | amount |
|----------------|----------|--------|
| 2              | 5        | 310    |
| 3              | 5        | 300    |
| 9              | 5        | 200    |
| 12             | 1        | 910    |
| 13             | 2        | 970    |

### Output

| customer_id | count_no_trans |
|-------------|----------------|
| 54          | 2              |
| 30          | 1              |
| 96          | 1              |

---

## Explanation

- **Customer 23**: Visited once and made one transaction.
- **Customer 9**: Visited once and made one transaction.
- **Customer 30**: Visited once and made no transactions.
- **Customer 54**: Visited three times, made no transactions during two visits, and made three transactions during one visit.
- **Customer 96**: Visited once and made no transactions.

---

## Solution

### SQL Query

```sql
SELECT 
    Visits.customer_id, 
    COUNT(Visits.visit_id) AS count_no_trans
FROM 
    Visits 
LEFT JOIN 
    Transactions
ON 
    Visits.visit_id = Transactions.visit_id
WHERE 
    Transactions.transaction_id IS NULL
GROUP BY 
    Visits.customer_id;
