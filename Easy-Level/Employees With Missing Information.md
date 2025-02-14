## Employees with Missing Information

### Table: Employees

| Column Name | Type    |
|-------------|---------|
| employee_id | int     |
| name        | varchar |

- `employee_id` is the column with unique values for this table.
- Each row of this table indicates the name of the employee whose ID is `employee_id`.

### Table: Salaries

| Column Name | Type    |
|-------------|---------|
| employee_id | int     |
| salary      | int     |

- `employee_id` is the column with unique values for this table.
- Each row of this table indicates the salary of the employee whose ID is `employee_id`.

### Problem Statement
Write a solution to report the IDs of all the employees with missing information. The information of an employee is missing if:

- The employee's name is missing, or
- The employee's salary is missing.

Return the result table ordered by `employee_id` in ascending order.

### Example
#### Input:
#### Employees table:

| employee_id | name     |
|-------------|----------|
| 2           | Crew     |
| 4           | Haven    |
| 5           | Kristian |

#### Salaries table:

| employee_id | salary |
|-------------|--------|
| 5           | 76071  |
| 1           | 22517  |
| 4           | 63539  |

#### Output:

| employee_id |
|-------------|
| 1           |
| 2           |

### Explanation
- Employees **1, 2, 4, and 5** are working at this company.
- The **name of employee 1** is missing.
- The **salary of employee 2** is missing.

### SQL Solution
```sql
SELECT employee_id FROM employees 
WHERE employees.employee_id NOT IN (SELECT salaries.employee_id FROM salaries) 
UNION 
SELECT salaries.employee_id FROM salaries 
WHERE salaries.employee_id NOT IN (SELECT employees.employee_id FROM employees) 
ORDER BY employee_id;
```

---