# Problem Description

You are given two tables: `Users` and `Register`. 

- The `Users` table contains information about users (`user_id` and `user_name`).
- The `Register` table contains data about which users have registered for which contests (`contest_id` and `user_id`).

Your task is to calculate the percentage of users who registered in each contest, rounded to two decimal places. The output should be sorted:
1. By `percentage` in descending order.
2. In case of a tie, by `contest_id` in ascending order.

---

## Example Input

### `Users` Table:
| user_id | user_name |
|---------|-----------|
| 6       | Alice     |
| 2       | Bob       |
| 7       | Alex      |

### `Register` Table:
| contest_id | user_id |
|------------|---------|
| 215        | 6       |
| 209        | 2       |
| 208        | 2       |
| 210        | 6       |
| 208        | 6       |
| 209        | 7       |
| 209        | 6       |
| 215        | 7       |
| 208        | 7       |
| 210        | 2       |
| 207        | 2       |
| 210        | 7       |

---

## Example Output

| contest_id | percentage |
|------------|------------|
| 208        | 100.0      |
| 209        | 100.0      |
| 210        | 100.0      |
| 215        | 66.67      |
| 207        | 33.33      |

---

## Explanation

1. **Contests 208, 209, and 210:** All users registered, so the percentage is 100%.
2. **Contest 215:** 2 out of 3 users registered, giving a percentage of `66.67%`.
3. **Contest 207:** Only 1 user out of 3 registered, giving a percentage of `33.33%`.

The output is sorted by `percentage` in descending order, with ties resolved by `contest_id` in ascending order.

---

# Solution

### SQL Query

```sql
SELECT 
    contest_id,
    ROUND((COUNT(user_id) / (SELECT COUNT(*) FROM Users)) * 100, 2) AS percentage
FROM 
    Register
GROUP BY 
    contest_id
ORDER BY 
    percentage DESC, 
    contest_id ASC;
