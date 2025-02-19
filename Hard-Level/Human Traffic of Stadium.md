# Stadium Table Query

## Table: Stadium

| Column Name  | Type  |
|-------------|-------|
| id          | int   |
| visit_date  | date  |
| people      | int   |

- `visit_date` is the column with unique values for this table.
- Each row of this table contains the visit date and visit id of the stadium with the number of people during the visit.
- As `id` increases, the date increases as well.

## Problem Statement

Write a solution to display the records where:
- There are three or more rows with **consecutive `id` values**.
- The number of people is **greater than or equal to 100** for each row.

The result table should be ordered by `visit_date` in **ascending order**.

---

### **Example 1**

#### **Input:**  
**Stadium table:**
| id  | visit_date | people |
|-----|------------|--------|
| 1   | 2017-01-01 | 10     |
| 2   | 2017-01-02 | 109    |
| 3   | 2017-01-03 | 150    |
| 4   | 2017-01-04 | 99     |
| 5   | 2017-01-05 | 145    |
| 6   | 2017-01-06 | 1455   |
| 7   | 2017-01-07 | 199    |
| 8   | 2017-01-09 | 188    |

#### **Output:**  
| id  | visit_date | people |
|-----|------------|--------|
| 5   | 2017-01-05 | 145    |
| 6   | 2017-01-06 | 1455   |
| 7   | 2017-01-07 | 199    |
| 8   | 2017-01-09 | 188    |

#### **Explanation:**  
- The four rows with `id`s **5, 6, 7, and 8** have **consecutive `id`s**, and each has `>= 100` people.
- The rows with `id`s **2 and 3** are not included because we need **at least three consecutive `id`s**.

---

## **Solution Query**
```sql
WITH tst AS (
    SELECT 
        id,
        visit_date,
        people,
        LAG(id, 1) OVER (ORDER BY visit_date) AS prev_id,
        LAG(id, 2) OVER (ORDER BY visit_date) AS prev_2_id,
        LEAD(id, 1) OVER (ORDER BY visit_date) AS next_id,
        LEAD(id, 2) OVER (ORDER BY visit_date) AS next_2_id
    FROM Stadium 
    WHERE people >= 100
)
SELECT id, visit_date, people 
FROM tst 
WHERE 
    (next_id - id = 1 AND id - prev_id = 1)
    OR (next_id - id = 1 AND next_2_id - next_id = 1)
    OR (prev_id - prev_2_id = 1 AND id - prev_id = 1);
