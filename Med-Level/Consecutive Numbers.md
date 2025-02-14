# Problem: Find Consecutive Numbers in Logs Table

## Table: Logs

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| num         | varchar |

- `id` is the primary key for this table.
- `id` is an autoincrement column starting from 1.

## Objective

Find all numbers that appear at least three times consecutively.  
Return the result table in any order.

---

### Example

#### Input: Logs table
| id | num |
|----|-----|
| 1  | 1   |
| 2  | 1   |
| 3  | 1   |
| 4  | 2   |
| 5  | 1   |
| 6  | 2   |
| 7  | 2   |

#### Output:
| ConsecutiveNums |
|-----------------|
| 1               |

#### Explanation:
- `1` is the only number that appears consecutively for at least three times.

---

## Solutions

### Solution 1: Using Self-Join (MySQL < 8.0)

```sql
SELECT DISTINCT logs.num AS ConsecutiveNums
FROM logs
JOIN logs l1 ON logs.id = l1.id - 1
JOIN logs l2 ON logs.id = l2.id - 2
WHERE logs.num = l1.num AND logs.num = l2.num;
