# Problem Description

### Table: Triangle

| Column Name | Type |
|-------------|------|
| x           | int  |
| y           | int  |
| z           | int  |

- `(x, y, z)` is the primary key for this table.
- Each row in the table represents the lengths of three line segments.

### Problem Statement

For each set of three line segments `(x, y, z)`, determine whether they can form a triangle or not. 

**Rules for forming a triangle:**
1. The sum of any two sides must be greater than the third side:
   - \(x + y > z\)
   - \(x + z > y\)
   - \(y + z > x\)

**Output Requirements:**
- Return a table with the columns `x`, `y`, `z`, and `triangle`.
- The `triangle` column should have:
  - `'Yes'` if the three segments form a triangle.
  - `'No'` otherwise.

---

### Example Input

**Triangle Table:**

| x  | y  | z  |
|----|----|----|
| 13 | 15 | 30 |
| 10 | 20 | 15 |

---

### Example Output

| x  | y  | z  | triangle |
|----|----|----|----------|
| 13 | 15 | 30 | No       |
| 10 | 20 | 15 | Yes      |

**Explanation:**
- For `(13, 15, 30)`: \(13 + 15 \not> 30\), so the segments cannot form a triangle.
- For `(10, 20, 15)`: All conditions are satisfied, so the segments form a triangle.

---

### SQL Solution

```sql
SELECT 
    x,
    y,
    z,
    IF(x + y > z AND x + z > y AND y + z > x, 'Yes', 'No') AS triangle
FROM 
    Triangle;
