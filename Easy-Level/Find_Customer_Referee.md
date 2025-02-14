# SQL Problem: Customers Not Referred by Customer with ID = 2

## Problem Description

Given a table `Customer`:

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
| referee_id  | int     |

- `id` is the primary key column for this table.
- Each row in this table represents a customer with their `id`, `name`, and the `id` of the customer who referred them (via the `referee_id` column).

The task is to find the **names** of customers who **were not referred by the customer with `id = 2`**.

### Result Format
The result should return the `name` of all matching customers in any order.

---

## Example

### Input
`Customer` table:

| id | name | referee_id |
|----|------|------------|
| 1  | Will | null       |
| 2  | Jane | null       |
| 3  | Alex | 2          |
| 4  | Bill | null       |
| 5  | Zack | 1          |
| 6  | Mark | 2          |

### Output
| name |
|------|
| Will |
| Jane |
| Bill |
| Zack |

### Explanation
Customers `Alex` and `Mark` have `referee_id = 2`, meaning they were referred by customer with `id = 2`. All other customers (`Will`, `Jane`, `Bill`, `Zack`) were not referred by customer `id = 2`.

---

## Solution

```sql
SELECT name  
FROM Customer  
WHERE referee_id != 2 OR referee_id IS NULL;
