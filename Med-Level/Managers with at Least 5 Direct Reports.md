# Find Managers with At Least Five Direct Reports

## Problem Description

### Table: Employee

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |

- `id` is the primary key (column with unique values) for this table.
- Each row of this table indicates the name of an employee, their department, and the `id` of their manager.
- If `managerId` is `NULL`, then the employee does not have a manager.
- No employee will be the manager of themselves.

## Task
Write a solution to find managers with at least five direct reports.

### Input
Example `Employee` table:

| id  | name  | department | managerId |
|-----|-------|------------|-----------|
| 101 | John  | A          | NULL      |
| 102 | Dan   | A          | 101       |
| 103 | James | A          | 101       |
| 104 | Amy   | A          | 101       |
| 105 | Anne  | A          | 101       |
| 106 | Ron   | B          | 101       |

### Output
Result table:

| name |
|------|
| John |

## SQL Solution

```sql
SELECT Employee.name
FROM Employee
JOIN Employee e1 ON Employee.id = e1.managerId
GROUP BY Employee.id, Employee.name
HAVING COUNT(e1.id) >= 5;
