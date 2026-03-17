# 3657. Find Loyal Customers

**Status:** Solved  
**Difficulty:** Medium  

---

## Table: customer_transactions

| Column Name      | Type    |
|-----------------|---------|
| transaction_id   | int     |
| customer_id      | int     |
| transaction_date | date    |
| amount           | decimal |
| transaction_type | varchar |

- `transaction_id` is the unique identifier for this table.  
- `transaction_type` can be either `'purchase'` or `'refund'`.  

Each row represents a customer transaction.

---

## Problem Description

A customer is considered **loyal** if they meet **ALL** the following criteria:

1. Made at least **3 purchase transactions**.  
2. Have been **active for at least 30 days**.  
3. Their **refund rate** is less than **20%**.  

**Refund rate** is calculated as:

\[
\text{refund rate} = \frac{\text{number of refund transactions}}{\text{total number of transactions (purchases + refunds)}}
\]

Return the result table **ordered by customer_id in ascending order**.

---

## Example

**Input: customer_transactions table**

| transaction_id | customer_id | transaction_date | amount | transaction_type |
|----------------|-------------|-----------------|--------|-----------------|
| 1              | 101         | 2024-01-05      | 150.00 | purchase        |
| 2              | 101         | 2024-01-15      | 200.00 | purchase        |
| 3              | 101         | 2024-02-10      | 180.00 | purchase        |
| 4              | 101         | 2024-02-20      | 250.00 | purchase        |
| 5              | 102         | 2024-01-10      | 100.00 | purchase        |
| 6              | 102         | 2024-01-12      | 120.00 | purchase        |
| 7              | 102         | 2024-01-15      | 80.00  | refund          |
| 8              | 102         | 2024-01-18      | 90.00  | refund          |
| 9              | 102         | 2024-02-15      | 130.00 | purchase        |
| 10             | 103         | 2024-01-01      | 500.00 | purchase        |
| 11             | 103         | 2024-01-02      | 450.00 | purchase        |
| 12             | 103         | 2024-01-03      | 400.00 | purchase        |
| 13             | 104         | 2024-01-01      | 200.00 | purchase        |
| 14             | 104         | 2024-02-01      | 250.00 | purchase        |
| 15             | 104         | 2024-02-15      | 300.00 | purchase        |
| 16             | 104         | 2024-03-01      | 350.00 | purchase        |
| 17             | 104         | 2024-03-10      | 280.00 | purchase        |
| 18             | 104         | 2024-03-15      | 100.00 | refund          |

---

**Output**

| customer_id |
|-------------|
| 101         |
| 104         |

**Explanation:**

- **Customer 101**: 4 purchases, 0 refunds, active for 46 days → loyal  
- **Customer 102**: 3 purchases, 2 refunds → refund rate = 40% → not loyal  
- **Customer 103**: 3 purchases, active for only 2 days → not loyal  
- **Customer 104**: 5 purchases, 1 refund, active for 73 days → refund rate = 16.67% → loyal  

---

## Solution (MySQL)

```sql
SELECT 
    customer_id
FROM customer_transactions
GROUP BY customer_id
HAVING 
    DATEDIFF(MAX(transaction_date), MIN(transaction_date)) >= 30
    AND SUM(CASE WHEN transaction_type='refund' THEN 1 ELSE 0 END)/COUNT(*) < 0.2
    AND SUM(CASE WHEN transaction_type='purchase' THEN 1 ELSE 0 END) >= 3;