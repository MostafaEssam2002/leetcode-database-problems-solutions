# Problem Description

## Table: Products

| Column Name   | Type    |
|---------------|---------|
| product_id    | int     |
| new_price     | int     |
| change_date   | date    |

- `(product_id, change_date)` is the primary key (combination of columns with unique values) of this table.
- Each row of this table indicates that the price of some product was changed to a new price at some date.

### Task:
Write a solution to find the prices of all products on **2019-08-16**. Assume the price of all products before any change is **10**.

Return the result table in any order.

---

## Example 1:

### Input: 

Products table:

| product_id | new_price | change_date |
|------------|-----------|-------------|
| 1          | 20        | 2019-08-14  |
| 2          | 50        | 2019-08-14  |
| 1          | 30        | 2019-08-15  |
| 1          | 35        | 2019-08-16  |
| 2          | 65        | 2019-08-17  |
| 3          | 20        | 2019-08-18  |

### Output: 

| product_id | price |
|------------|-------|
| 2          | 50    |
| 1          | 35    |
| 3          | 10    |

---

## Solution:

```sql
WITH RankedPrices AS (
    SELECT 
        product_id, 
        new_price, 
        change_date,
        RANK() OVER (PARTITION BY product_id ORDER BY change_date DESC) AS rnk
    FROM Products
    WHERE change_date <= '2019-08-16'
)
SELECT 
    p.product_id, 
    COALESCE(rp.new_price, 10) AS price
FROM (SELECT DISTINCT product_id FROM Products) p
LEFT JOIN RankedPrices rp
ON p.product_id = rp.product_id AND rp.rnk = 1;
