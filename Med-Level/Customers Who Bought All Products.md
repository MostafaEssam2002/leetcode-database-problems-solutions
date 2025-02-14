# Problem Description

## Table: Customer

| Column Name  | Type |
|--------------|------|
| customer_id  | int  |
| product_key  | int  |

- This table may contain duplicate rows.
- `customer_id` is not `NULL`.
- `product_key` is a foreign key (reference column) to the `Product` table.

## Table: Product

| Column Name  | Type |
|--------------|------|
| product_key  | int  |

- `product_key` is the primary key (column with unique values) for this table.

---

## Task

Write a solution to report the `customer_id`s from the `Customer` table that bought **all the products** in the `Product` table.

- Return the result table in any order.

---

## Example

### Input:
**Customer table:**

| customer_id | product_key |
|-------------|-------------|
| 1           | 5           |
| 2           | 6           |
| 3           | 5           |
| 3           | 6           |
| 1           | 6           |

**Product table:**

| product_key |
|-------------|
| 5           |
| 6           |

### Output:
**Result table:**

| customer_id |
|-------------|
| 1           |
| 3           |

### Explanation:
The customers who bought all the products (5 and 6) are customers with IDs `1` and `3`.

---

# SQL Solution

```sql
SELECT customer_id  
FROM customer 
JOIN product ON customer.product_key = product.product_key 
GROUP BY customer.customer_id 
HAVING COUNT(DISTINCT customer.product_key) = (SELECT COUNT(*) FROM product);
