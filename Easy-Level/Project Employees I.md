## Problem Description

We are given two tables:

### Project Table:
| Column Name   | Type   |
|---------------|--------|
| project_id    | int    |
| employee_id   | int    |

- `project_id` and `employee_id` together form the primary key of this table.
- `employee_id` is a foreign key to the Employee table.
- Each row in the Project table indicates that an employee with `employee_id` is working on the project with `project_id`.

### Employee Table:
| Column Name     | Type    |
|-----------------|---------|
| employee_id     | int     |
| name            | varchar |
| experience_years| int     |

- `employee_id` is the primary key of the Employee table.
- It's guaranteed that `experience_years` is not NULL.
- Each row in this table contains information about one employee.

### Task:
Write an SQL query to calculate the average `experience_years` for each project, rounded to two decimal places. If a project has no employees assigned, it should return an average of `0` for that project.

### Example:

#### Input:
**Project Table**:

| project_id | employee_id |
|------------|-------------|
| 1          | 1           |
| 1          | 2           |
| 1          | 3           |
| 2          | 1           |
| 2          | 4           |

**Employee Table**:

| employee_id | name   | experience_years |
|-------------|--------|------------------|
| 1           | Khaled | 3                |
| 2           | Ali    | 2                |
| 3           | John   | 1                |
| 4           | Doe    | 2                |

#### Output:
| project_id | average_years |
|------------|---------------|
| 1          | 2.00          |
| 2          | 2.50          |

#### Explanation:
- For project 1, the experience years are (3 + 2 + 1) / 3 = 2.00.
- For project 2, the experience years are (3 + 2) / 2 = 2.50.

## Solution:

To solve this, we will perform the following steps:

1. **Join** the `Project` table with the `Employee` table using the `employee_id`.
2. **Calculate the average** of `experience_years` for each project.
3. **Round the result** to two decimal places and handle cases where there are no employees assigned to a project by using `IFNULL` to return 0.
4. **Group** the results by `project_id`.

### SQL Query:

```sql
SELECT project_id, ROUND(IFNULL(AVG(experience_years), 0), 2) AS average_years
FROM (
    SELECT p.project_id, e.experience_years
    FROM project p
    LEFT JOIN employee e ON p.employee_id = e.employee_id
) AS tst
GROUP BY project_id;
