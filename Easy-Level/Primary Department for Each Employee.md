# Problem Description

### Table: Employee

| Column Name   | Type    |
|---------------|---------|
| employee_id   | int     |
| department_id | int     |
| primary_flag  | varchar |

- `(employee_id, department_id)` is the primary key for this table, meaning the combination of these columns has unique values.
- `employee_id` is the ID of the employee.
- `department_id` is the ID of the department to which the employee belongs.
- `primary_flag` is an ENUM with values `'Y'` or `'N'`:
  - `'Y'` indicates the department is the **primary department** for the employee.
  - `'N'` indicates it is **not the primary department**.

### Problem Statement
- Employees can belong to multiple departments.
- If an employee belongs to only one department, their `primary_flag` is `'N'`, and this department is considered their primary department.
- Write a solution to report all employees with their primary department.
- If an employee belongs to only one department, report that department as their primary.

### Example Input

**Employee table:**

| employee_id | department_id | primary_flag |
|-------------|---------------|--------------|
| 1           | 1             | N            |
| 2           | 1             | Y            |
| 2           | 2             | N            |
| 3           | 3             | N            |
| 4           | 2             | N            |
| 4           | 3             | Y            |
| 4           | 4             | N            |

### Example Output

| employee_id | department_id |
|-------------|---------------|
| 1           | 1             |
| 2           | 1             |
| 3           | 3             |
| 4           | 3             |

### Explanation
- Employee `1` belongs to only one department, so department `1` is their primary.
- Employee `2` has department `1` as their primary (`primary_flag = 'Y'`).
- Employee `3` belongs to only one department, so department `3` is their primary.
- Employee `4` has department `3` as their primary (`primary_flag = 'Y'`).

---

# SQL Solution

```sql
SELECT 
    employee_id,
    department_id
FROM 
    Employee
WHERE 
    primary_flag = 'Y' -- Select employees with an explicitly defined primary department
    OR employee_id NOT IN (
        SELECT employee_id 
        FROM Employee 
        WHERE primary_flag = 'Y'
    );  
