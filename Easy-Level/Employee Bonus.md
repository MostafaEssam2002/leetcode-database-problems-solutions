# SQL Challenge: Employee Bonus   

## Problem Description

You are given two tables: `Employee` and `Bonus`.

### Employee Table:
| Column Name | Type    | Description                               |
|-------------|---------|-------------------------------------------|
| empId       | int     | The unique ID of each employee.           |
| name        | varchar | The name of the employee.                 |
| supervisor  | int     | The ID of the employee's supervisor.      |
| salary      | int     | The salary of the employee.               |

### Bonus Table:
| Column Name | Type    | Description                               |
|-------------|---------|-------------------------------------------|
| empId       | int     | The ID of the employee (foreign key).     |
| bonus       | int     | The bonus amount of the employee.         |

The `empId` column in the `Bonus` table is a foreign key referencing the `empId` column in the `Employee` table.

### Task:
Write a SQL query to report the `name` and `bonus` amount of each employee:
- Include employees with a bonus amount less than 1000.
- Include employees who do not have a bonus (null values).

Return the result in any order.

---

## Example Input

### Employee Table:
| empId | name   | supervisor | salary |
|-------|--------|------------|--------|
| 3     | Brad   | null       | 4000   |
| 1     | John   | 3          | 1000   |
| 2     | Dan    | 3          | 2000   |
| 4     | Thomas | 3          | 4000   |

### Bonus Table:
| empId | bonus |
|-------|-------|
| 2     | 500   |
| 4     | 2000  |

---

## Expected Output
| name   | bonus |
|--------|-------|
| Brad   | null  |
| John   | null  |
| Dan    | 500   |

---

## Solution

### SQL Query:

```sql
SELECT 
    name, 
    bonus 
FROM 
    Employee 
LEFT JOIN 
    Bonus 
ON 
    Employee.empId = Bonus.empId 
WHERE 
    bonus < 1000 OR bonus IS NULL;
