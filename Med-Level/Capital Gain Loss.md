# Stock Capital Gain/Loss Report

## Problem Description

### Table: Stocks

| Column Name   | Type    |
|--------------|---------|
| stock_name   | varchar |
| operation    | enum    |
| operation_day| int     |
| price        | int     |

- **(stock_name, operation_day) is the primary key** (a unique combination of columns).
- The **operation** column is an **ENUM** with values **('Sell', 'Buy')**.
- Each row represents a stock operation performed on a specific day.
- It is **guaranteed** that:
  - Each **"Sell"** operation has a corresponding **"Buy"** operation on a previous day.
  - Each **"Buy"** operation has a corresponding **"Sell"** operation on a later day.

---

### Task:
Write an SQL query to **calculate the total capital gain/loss** for each stock.

- The **Capital gain/loss** is determined by the total difference between the selling price and the buying price across multiple transactions.

Return the result table **in any order**.

---

## Example:

### Example 1:

#### Input:
**Stocks table:**

| stock_name   | operation | operation_day | price  |
|-------------|-----------|---------------|--------|
| Leetcode    | Buy       | 1             | 1000   |
| Corona Masks| Buy       | 2             | 10     |
| Leetcode    | Sell      | 5             | 9000   |
| Handbags    | Buy       | 17            | 30000  |
| Corona Masks| Sell      | 3             | 1010   |
| Corona Masks| Buy       | 4             | 1000   |
| Corona Masks| Sell      | 5             | 500    |
| Corona Masks| Buy       | 6             | 1000   |
| Handbags    | Sell      | 29            | 7000   |
| Corona Masks| Sell      | 10            | 10000  |

---

#### Output:

| stock_name   | capital_gain_loss |
|-------------|-------------------|
| Corona Masks| 9500              |
| Leetcode    | 8000              |
| Handbags    | -23000            |

---

#### Explanation:
1. **Leetcode stock:**
   - Bought on **day 1** for **$1000**.
   - Sold on **day 5** for **$9000**.
   - **Capital gain** = **9000 - 1000 = 8000**.

2. **Handbags stock:**
   - Bought on **day 17** for **$30000**.
   - Sold on **day 29** for **$7000**.
   - **Capital loss** = **7000 - 30000 = -23000**.

3. **Corona Masks stock:**
   - Bought on **day 2** for **$10** → Sold on **day 3** for **$1010** → **Gain: 1010 - 10 = 1000**.
   - Bought on **day 4** for **$1000** → Sold on **day 5** for **$500** → **Loss: 500 - 1000 = -500**.
   - Bought on **day 6** for **$1000** → Sold on **day 10** for **$10000** → **Gain: 10000 - 1000 = 9000**.
   - **Total gain/loss** = **1000 - 500 + 9000 = 9500**.

---

## Solution:

```sql
WITH temp_stocks AS (
    SELECT stock_name,
           SUM(CASE WHEN operation = 'Sell' THEN price ELSE 0 END) AS tot_sell,
           SUM(CASE WHEN operation = 'Buy' THEN price ELSE 0 END) AS tot_buy
    FROM stocks
    GROUP BY stock_name
)
SELECT stock_name, 
       (COALESCE(tot_sell, 0) - COALESCE(tot_buy, 0)) AS capital_gain_loss  
FROM temp_stocks;
