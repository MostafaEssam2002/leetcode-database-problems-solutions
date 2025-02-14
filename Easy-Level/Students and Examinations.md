# SQL Problem: Count the Number of Times Each Student Attended Each Exam

## Problem Description

You have three tables:

### `Students` Table:

| Column Name   | Type    |
|---------------|---------|
| student_id    | int     |
| student_name  | varchar |

`student_id` is the primary key (column with unique values) for this table. Each row of this table contains the ID and the name of one student in the school.

### `Subjects` Table:

| Column Name  | Type    |
|--------------|---------|
| subject_name | varchar |

`subject_name` is the primary key (column with unique values) for this table. Each row of this table contains the name of one subject in the school.

### `Examinations` Table:

| Column Name  | Type    |
|--------------|---------|
| student_id   | int     |
| subject_name | varchar |

There is no primary key (column with unique values) for this table. It may contain duplicates. Each student from the `Students` table takes every course from the `Subjects` table. Each row of this table indicates that a student with ID `student_id` attended the exam of `subject_name`.

## Objective

Write a query to find the number of times each student attended each exam. 

Return the result table ordered by `student_id` and `subject_name`.

### Example

**Input:**

`Students` Table:

| student_id | student_name |
|------------|--------------|
| 1          | Alice        |
| 2          | Bob          |
| 13         | John         |
| 6          | Alex         |

`Subjects` Table:

| subject_name |
|--------------|
| Math         |
| Physics      |
| Programming  |

`Examinations` Table:

| student_id | subject_name |
|------------|--------------|
| 1          | Math         |
| 1          | Physics      |
| 1          | Programming  |
| 2          | Programming  |
| 1          | Physics      |
| 1          | Math         |
| 13         | Math         |
| 13         | Programming  |
| 13         | Physics      |
| 2          | Math         |
| 1          | Math         |

**Output:**

| student_id | student_name | subject_name | attended_exams |
|------------|--------------|--------------|----------------|
| 1          | Alice        | Math         | 3              |
| 1          | Alice        | Physics      | 2              |
| 1          | Alice        | Programming  | 1              |
| 2          | Bob          | Math         | 1              |
| 2          | Bob          | Physics      | 0              |
| 2          | Bob          | Programming  | 1              |
| 6          | Alex         | Math         | 0              |
| 6          | Alex         | Physics      | 0              |
| 6          | Alex         | Programming  | 0              |
| 13         | John         | Math         | 1              |
| 13         | John         | Physics      | 1              |
| 13         | John         | Programming  | 1              |

**Explanation:**

The result table should contain all students and all subjects. 
- Alice attended the Math exam 3 times, the Physics exam 2 times, and the Programming exam 1 time.
- Bob attended the Math exam 1 time, the Programming exam 1 time, and did not attend the Physics exam.
- Alex did not attend any exams.
- John attended the Math exam 1 time, the Physics exam 1 time, and the Programming exam 1 time.

## SQL Query

```sql
SELECT 
    Students.student_id, 
    Students.student_name, 
    Subjects.subject_name, 
    COUNT(Examinations.subject_name) AS attended_exams
FROM 
    Students
JOIN 
    Subjects
LEFT JOIN 
    Examinations 
    ON Examinations.student_id = Students.student_id 
    AND Examinations.subject_name = Subjects.subject_name
GROUP BY 
    Students.student_id, Students.student_name, Subjects.subject_name
ORDER BY 
    Students.student_id, Subjects.subject_name;
