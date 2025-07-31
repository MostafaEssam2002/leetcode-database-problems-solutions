# Find Product Recommendation Pairs

## Problem Description

Amazon wants to implement the "Customers who bought this also bought..." feature based on co-purchase patterns. The goal is to identify product pairs that are frequently purchased together by the same customers.

## Database Schema

### Table: ProductPurchases

| Column Name | Type |
|-------------|------|
| user_id     | int  |
| product_id  | int  |
| quantity    | int  |

- `(user_id, product_id)` is the unique key for this table
- Each row represents a purchase of a product by a user in a specific quantity

### Table: ProductInfo

| Column Name | Type    |
|-------------|---------|
| product_id  | int     |
| category    | varchar |
| price       | decimal |

- `product_id` is the primary key for this table
- Each row assigns a category and price to a product

## Requirements

Write a SQL solution to:

1. **Identify distinct product pairs** frequently purchased together by the same customers (where `product1_id < product2_id`)
2. **Count customers** who purchased both products in each pair
3. **Filter recommendations** - only include product pairs purchased by at least 3 different customers
4. **Sort results** by:
   - `customer_count` in descending order
   - In case of tie: `product1_id` in ascending order
   - Then by: `product2_id` in ascending order

## Example

### Input Data

**ProductPurchases table:**

| user_id | product_id | quantity |
|---------|------------|----------|
| 1       | 101        | 2        |
| 1       | 102        | 1        |
| 1       | 103        | 3        |
| 2       | 101        | 1        |
| 2       | 102        | 5        |
| 2       | 104        | 1        |
| 3       | 101        | 2        |
| 3       | 103        | 1        |
| 3       | 105        | 4        |
| 4       | 101        | 1        |
| 4       | 102        | 1        |
| 4       | 103        | 2        |
| 4       | 104        | 3        |
| 5       | 102        | 2        |
| 5       | 104        | 1        |

**ProductInfo table:**

| product_id | category    | price |
|------------|-------------|-------|
| 101        | Electronics | 100   |
| 102        | Books       | 20    |
| 103        | Clothing    | 35    |
| 104        | Kitchen     | 50    |
| 105        | Sports      | 75    |

### Expected Output

| product1_id | product2_id | product1_category | product2_category | customer_count |
|-------------|-------------|-------------------|-------------------|----------------|
| 101         | 102         | Electronics       | Books             | 3              |
| 101         | 103         | Electronics       | Clothing          | 3              |
| 102         | 104         | Books             | Kitchen           | 3              |

### Explanation

- **Product pair (101, 102)**: Purchased by users 1, 2, and 4 (3 customers)
- **Product pair (101, 103)**: Purchased by users 1, 3, and 4 (3 customers)  
- **Product pair (102, 104)**: Purchased by users 2, 4, and 5 (3 customers)

All pairs meet the minimum threshold of 3 customers and are ordered by the specified criteria.
## Solution Breakdown

1. **CTE (Common Table Expression)**: Creates all valid product pairs for each user
   - Self-joins `ProductPurchases` on `user_id`
   - Ensures `product1_id < product2_id` to avoid duplicates
   - Joins with `ProductInfo` to get category information

2. **Main Query**: Aggregates and filters the results
   - Groups by product pair and categories
   - Counts distinct customers per pair
   - Filters pairs with at least 3 customers
   - Orders results as specified

## Key Concepts

- **Self-join** to find product pairs purchased by same user
- **Conditional join** (`p1.product_id < p2.product_id`) to avoid duplicate pairs
- **Aggregation** with `GROUP BY` and `COUNT()`
- **Filtering** with `HAVING` clause
- **Multi-level sorting** with `ORDER BY`

## SQL Solution

```sql
WITH pairs AS (
    SELECT 
        p1.user_id,
        pf1.category AS product1_category,
        pf2.category AS product2_category,
        p1.product_id AS product1_id,
        p2.product_id AS product2_id
    FROM productpurchases p1
    JOIN productpurchases p2 
        ON p1.user_id = p2.user_id 
       AND p1.product_id < p2.product_id
    JOIN productinfo pf1 ON pf1.product_id = p1.product_id 
    JOIN productinfo pf2 ON pf2.product_id = p2.product_id 
) 
SELECT 
    product1_id,
    product2_id,
    product1_category,
    product2_category,
    COUNT(user_id) AS customer_count
FROM pairs 
GROUP BY product1_id, product2_id, product1_category, product2_category
HAVING customer_count >= 3
ORDER BY customer_count DESC, product1_id ASC, product2_id ASC;
```

