# Problem Description

The `Employees` table contains the following columns:

| Column Name | Type     |
|-------------|----------|
| employee_id | int      |
| name        | varchar  |
| reports_to  | int      |
| age         | int      |

- `employee_id` is the primary key of the table.
- This table contains information about employees and the IDs of the managers they report to.
- Some employees do not report to anyone (`reports_to` is `NULL`).

A manager is defined as an employee who has at least one other employee reporting to them.

## Task
Write an SQL query to report:
- The IDs and names of all managers.
- The number of employees who report directly to each manager.
- The average age of those employees, rounded to the nearest integer.

The result should be ordered by `employee_id`.

### Example 1

Input: 

| employee_id | name    | reports_to | age |
|-------------|---------|------------|-----|
| 9           | Hercy   | NULL       | 43  |
| 6           | Alice   | 9          | 41  |
| 4           | Bob     | 9          | 36  |
| 2           | Winston | NULL       | 37  |

Output:

| employee_id | name  | reports_count | average_age |
|-------------|-------|---------------|-------------|
| 9           | Hercy | 2             | 39          |

Explanation: Hercy has 2 employees (Alice and Bob) reporting to them. Their average age is `(41 + 36) / 2 = 38.5`, rounded to 39.

### Example 2

Input: 

| employee_id | name    | reports_to | age |
|-------------|---------|------------|-----|
| 1           | Michael | NULL       | 45  |
| 2           | Alice   | 1          | 38  |
| 3           | Bob     | 1          | 42  |
| 4           | Charlie | 2          | 34  |
| 5           | David   | 2          | 40  |
| 6           | Eve     | 3          | 37  |
| 7           | Frank   | NULL       | 50  |
| 8           | Grace   | NULL       | 48  |

Output:

| employee_id | name    | reports_count | average_age |
|-------------|---------|---------------|-------------|
| 1           | Michael | 2             | 40          |
| 2           | Alice   | 2             | 37          |
| 3           | Bob     | 1             | 37          |

---

# Solution

Here’s the SQL query to solve the problem:

```sql
SELECT 
    employees.employee_id, 
    employees.name, 
    COUNT(e2.reports_to) AS reports_count, 
    ROUND(AVG(e2.age)) AS average_age
FROM 
    employees
LEFT JOIN 
    employees e2 
ON 
    employees.employee_id = e2.reports_to
WHERE 
    e2.reports_to IS NOT NULL
GROUP BY 
    employees.employee_id, employees.name
ORDER BY 
    employees.employee_id;
