# Problem Description

You are given a table `Teacher` that contains information about teachers and the subjects they teach in various departments.

- The `Teacher` table contains the following columns:
  - `teacher_id`: ID of the teacher.
  - `subject_id`: ID of the subject.
  - `dept_id`: ID of the department.

Each row in the table indicates that the teacher with `teacher_id` teaches the subject `subject_id` in the department `dept_id`.

Your task is to calculate the number of unique subjects each teacher teaches in the university. The output should contain `teacher_id` and the count of unique subjects they teach (`cnt`).

---

## Example Input

### `Teacher` Table:

| teacher_id | subject_id | dept_id |
|------------|------------|---------|
| 1          | 2          | 3       |
| 1          | 2          | 4       |
| 1          | 3          | 3       |
| 2          | 1          | 1       |
| 2          | 2          | 1       |
| 2          | 3          | 1       |
| 2          | 4          | 1       |

---

## Example Output

| teacher_id | cnt |
|------------|-----|
| 1          | 2   |
| 2          | 4   |

---

## Explanation

1. **Teacher 1**:
   - They teach subject 2 in departments 3 and 4.
   - They teach subject 3 in department 3.
   - Therefore, they teach 2 unique subjects.
2. **Teacher 2**:
   - They teach subject 1, 2, 3, and 4 in department 1.
   - Therefore, they teach 4 unique subjects.

---

# Solution

### SQL Query

```sql
SELECT teacher_id, COUNT(DISTINCT subject_id) AS cnt
FROM Teacher
GROUP BY teacher_id;
