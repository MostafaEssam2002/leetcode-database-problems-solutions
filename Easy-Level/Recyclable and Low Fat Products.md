# LeetCode MySQL Problem: Low Fat and Recyclable Products

## Problem Description

Given a table `Products`:

| Column Name | Type    |
|-------------|---------|
| product_id  | int     |
| low_fats    | enum    |
| recyclable  | enum    |

- `product_id` is the primary key (a unique column) for this table.
- `low_fats` is an `ENUM` with values ('Y', 'N'), where:
  - `'Y'` indicates the product is low fat.
  - `'N'` indicates the product is not low fat.
- `recyclable` is an `ENUM` with values ('Y', 'N'), where:
  - `'Y'` indicates the product is recyclable.
  - `'N'` indicates the product is not recyclable.

The task is to find the IDs of products that are **both low fat and recyclable**.

### Result Format
The result should return the `product_id` of the matching products in any order.

---

## Example

### Input
`Products` table:

| product_id  | low_fats | recyclable |
|-------------|----------|------------|
| 0           | Y        | N          |
| 1           | Y        | Y          |
| 2           | N        | Y          |
| 3           | Y        | Y          |
| 4           | N        | N          |

### Output
| product_id  |
|-------------|
| 1           |
| 3           |

### Explanation
Only products with IDs `1` and `3` meet the condition of being both low fat (`low_fats = 'Y'`) and recyclable (`recyclable = 'Y'`).

---

## Solution

```sql
SELECT product_id  
FROM Products  
WHERE low_fats = 'Y' AND recyclable = 'Y';
