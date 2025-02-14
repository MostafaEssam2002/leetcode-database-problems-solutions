# Odd and Even Transaction Sums Report

## Problem Description

### Table: Transactions

| Column Name      | Type |
|-----------------|------|
| transaction_id  | int  |
| amount          | int  |
| transaction_date| date |

- **transaction_id is the primary key** (each row has a unique ID).
- Each row represents a **transaction** with:
  - A **transaction ID**.
  - An **amount**.
  - The **date** of the transaction.

---

### Task:
Write an SQL query to **calculate the sum of amounts for odd and even transactions** for each day.

- If there are **no odd or even transactions** on a specific date, display **0**.
- **Order** the result by `transaction_date` **in ascending order**.

---

## Example:

### Example 1:

#### Input:

**Transactions table:**

| transaction_id | amount | transaction_date |
|---------------|--------|------------------|
| 1             | 150    | 2024-07-01       |
| 2             | 200    | 2024-07-01       |
| 3             | 75     | 2024-07-01       |
| 4             | 300    | 2024-07-02       |
| 5             | 50     | 2024-07-02       |
| 6             | 120    | 2024-07-03       |

---

#### Output:

| transaction_date | odd_sum | even_sum |
|------------------|---------|----------|
| 2024-07-01       | 75      | 350      |
| 2024-07-02       | 0       | 350      |
| 2024-07-03       | 0       | 120      |

---

#### Explanation:

For each `transaction_date`:
- **2024-07-01**:
  - **Odd transaction sums**: **75**
  - **Even transaction sums**: **150 + 200 = 350**
  
- **2024-07-02**:
  - **Odd transaction sums**: **0** (no odd numbers)
  - **Even transaction sums**: **300 + 50 = 350**

- **2024-07-03**:
  - **Odd transaction sums**: **0** (no odd numbers)
  - **Even transaction sums**: **120**

**The output is ordered by `transaction_date` in ascending order.**

---

## Solution:

```sql
SELECT transaction_date,
       SUM(CASE WHEN amount % 2 != 0 THEN amount ELSE 0 END) AS odd_sum,
       SUM(CASE WHEN amount % 2 = 0 THEN amount ELSE 0 END) AS even_sum
FROM transactions
GROUP BY transaction_date
ORDER BY transaction_date;
