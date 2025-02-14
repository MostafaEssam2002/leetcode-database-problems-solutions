## Table: Sales

| Column Name | Type  |
|-------------|-------|
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |

**Primary Key:** (sale_id, year) - Combination of columns with unique values.  
**Foreign Key:** `product_id` references the `Product` table.  

Each row of this table shows a sale of the product `product_id` in a certain year. Note that the `price` is per unit.

---

## Table: Product

| Column Name  | Type    |
|--------------|---------|
| product_id   | int     |
| product_name | varchar |

**Primary Key:** `product_id` - Column with unique values.  

Each row of this table indicates the product name of each product.

---

## Problem Statement:

Write a solution to select the product ID, year, quantity, and price for the **first year** of every product sold.

Return the resulting table in any order.

---

## Example:

### Input:

**Sales table:**

| sale_id | product_id | year | quantity | price |
|---------|------------|------|----------|-------|
| 1       | 100        | 2008 | 10       | 5000  |
| 2       | 100        | 2009 | 12       | 5000  |
| 7       | 200        | 2011 | 15       | 9000  |

**Product table:**

| product_id | product_name |
|------------|--------------|
| 100        | Nokia        |
| 200        | Apple        |
| 300        | Samsung      |

### Output:

| product_id | first_year | quantity | price |
|------------|------------|----------|-------|
| 100        | 2008       | 10       | 5000  |
| 200        | 2011       | 15       | 9000  |

---

## SQL Solution:

Here’s the SQL query to solve the problem:

```sql
WITH RankedSales AS (
    SELECT 
        product_id,
        year,
        quantity,
        price,
        ROW_NUMBER() OVER (PARTITION BY product_id ORDER BY year ASC) AS rn
    FROM sales
)
SELECT 
    rs.product_id, 
    rs.year AS first_year, 
    rs.quantity, 
    rs.price
FROM RankedSales rs
WHERE rs.rn = 1;
