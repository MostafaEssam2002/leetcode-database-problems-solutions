# Problem Description

You are given a table `Courses` that contains information about students and the classes they are enrolled in.

### Table: Courses

| Column Name | Type    |
|-------------|---------|
| student     | varchar |
| class       | varchar |

- `(student, class)` is the primary key, meaning each student-class pair is unique.
- Each row represents the name of a student and the class they are enrolled in.

---

## Task

Write a query to find all the classes that have at least five students.

---

## Example Input

### `Courses` Table:
| student | class    |
|---------|----------|
| A       | Math     |
| B       | English  |
| C       | Math     |
| D       | Biology  |
| E       | Math     |
| F       | Computer |
| G       | Math     |
| H       | Math     |
| I       | Math     |

---

## Example Output

| class   |
|---------|
| Math    |

### Explanation:
- The class `Math` has 6 students, so it is included in the output.
- The classes `English`, `Biology`, and `Computer` each have only 1 student, so they are not included.

---

## Solution

### SQL Query
```sql
SELECT class
FROM Courses
GROUP BY class
HAVING COUNT(student) >= 5;
