## Problem Description

You have a `Weather` table that contains temperature information for different dates. The task is to find all dates where the temperature is higher than the previous day's temperature. The result should return the `id` of the dates where this condition is met.

### Table Structure

| Column Name   | Type   |
|---------------|--------|
| `id`          | `int`  |
| `recordDate`  | `date` |
| `temperature` | `int`  |

- `id` is a unique identifier for each row.
- There are no rows with the same `recordDate`.
- The table contains temperature data for different dates.

### Example

#### Input:

`Weather` table:

| id  | recordDate | temperature |
|-----|------------|-------------|
| 1   | 2015-01-01 | 10          |
| 2   | 2015-01-02 | 25          |
| 3   | 2015-01-03 | 20          |
| 4   | 2015-01-04 | 30          |

#### Output:

| id  |
|-----|
| 2   |
| 4   |

#### Explanation:
- On **2015-01-02**, the temperature was higher than the previous day (10 → 25).
- On **2015-01-04**, the temperature was higher than the previous day (20 → 30).

## Solution

To solve this, we can use a **self join** approach to compare each day's temperature with the previous day's temperature.

### Steps:
1. **Join the table with itself**: Join the `Weather` table with itself where the `recordDate` of the first table (`W1`) is one day after the `recordDate` of the second table (`W2`).
2. **Check temperature comparison**: For each pair of rows, compare the temperatures and check if the temperature on the current day (`W1.temperature`) is greater than the temperature on the previous day (`W2.temperature`).
3. **Return the `id`**: For each pair that satisfies the condition, return the `id` of the current day (`W1.id`).

### SQL Query

```sql
SELECT W1.id
FROM Weather W1
JOIN Weather W2
  ON DATEDIFF(W1.recordDate, W2.recordDate) = 1
WHERE W1.temperature > W2.temperature;
