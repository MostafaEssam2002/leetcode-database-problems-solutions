# Nth Highest Salary

## Problem Description

### Table: Employee

| Column Name | Type  |
|-------------|-------|
| id          | int   |
| salary      | int   |

- `id` is the **primary key** (column with unique values) for this table.
- Each row of this table contains information about the **salary of an employee**.

### Task:
Write a solution to find the **nth highest salary** from the Employee table.  
If there is **no nth highest salary**, return `NULL`.

### Example:

#### Example 1:

##### Input:
**Employee table:**

| id | salary |
|----|--------|
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |

`n = 2`

##### Output:

| getNthHighestSalary(2) |
|------------------------|
| 200                    |

#### Example 2:

##### Input:
**Employee table:**

| id | salary |
|----|--------|
| 1  | 100    |

`n = 2`

##### Output:

| getNthHighestSalary(2) |
|------------------------|
| null                   |

#### Explanation:
- In **Example 1**, the **2nd highest salary** is **200**.
- In **Example 2**, there is **only one salary available**, so the **2nd highest salary does not exist**, and the output is **NULL**.

---

## Solution:

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
DETERMINISTIC
BEGIN
  DECLARE result INT;

  SET result = (
    SELECT salary FROM (
      SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
      FROM Employee
    ) ranked_salaries
    WHERE rnk = N
    LIMIT 1
  );

  RETURN result;
END;
