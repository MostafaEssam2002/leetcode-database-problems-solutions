## Problem: Find the second highest distinct salary from the Employee table.

### Table: Employee

| Column Name | Type |
|-------------|------|
| id          | int  |
| salary      | int  |

- `id` is the primary key (column with unique values) for this table.
- Each row of this table contains information about the salary of an employee.

### Goal:
Write a solution to find the second highest distinct salary from the Employee table. If there is no second highest salary, return `NULL` (or `None` in Pandas).

### Example:

#### Example 1:

**Input:**
Employee table:
| id  | salary |
|-----|--------|
| 1   | 100    |
| 2   | 200    |
| 3   | 300    |

**Output:**
| SecondHighestSalary |
|---------------------|
| 200                 |

#### Example 2:

**Input:**
Employee table:
| id  | salary |
|-----|--------|
| 1   | 100    |

**Output:**
| SecondHighestSalary |
|---------------------|
| NULL                |

### Solution:

To get the second highest distinct salary:

```sql
SELECT IFNULL(
    (SELECT MAX(employee.salary) 
     FROM employee 
     WHERE employee.salary < (SELECT MAX(employee.salary) FROM employee)),
    NULL) AS SecondHighestSalary;

-- Another Solution
select max(salary) as SecondHighestSalary from Employee where salary not in (select max(salary) from Employee)
