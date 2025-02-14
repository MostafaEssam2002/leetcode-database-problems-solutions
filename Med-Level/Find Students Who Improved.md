# Students Who Have Shown Improvement

## Problem Description

### Table: Scores

| Column Name | Type    |
|-------------|---------|
| student_id  | int     |
| subject     | varchar |
| score       | int     |
| exam_date   | varchar |

- **(student_id, subject, exam_date) is the primary key**, meaning:
  - Each row represents a unique record for a student's exam.
  - A student can have multiple exam records for the same subject on different dates.
- The **score** is between **0 and 100**.

---

### Task:
Write an SQL query to find students who have shown **improvement**. A student qualifies if:
1. They have taken **exams in the same subject on at least two different dates**.
2. Their **latest score is higher** than their **first score**.

- The result should be ordered by **student_id and subject in ascending order**.

---

## Example:

### Example 1:

#### Input:

**Scores table:**

| student_id | subject  | score | exam_date  |
|------------|----------|-------|------------|
| 101        | Math     | 70    | 2023-01-15 |
| 101        | Math     | 85    | 2023-02-15 |
| 101        | Physics  | 65    | 2023-01-15 |
| 101        | Physics  | 60    | 2023-02-15 |
| 102        | Math     | 80    | 2023-01-15 |
| 102        | Math     | 85    | 2023-02-15 |
| 103        | Math     | 90    | 2023-01-15 |
| 104        | Physics  | 75    | 2023-01-15 |
| 104        | Physics  | 85    | 2023-02-15 |

---

#### Output:

| student_id | subject  | first_score | latest_score |
|------------|----------|-------------|--------------|
| 101        | Math     | 70          | 85           |
| 102        | Math     | 80          | 85           |
| 104        | Physics  | 75          | 85           |

---

#### Explanation:

- **Student 101 (Math)**:
  - First score: **70**
  - Latest score: **85**
  - **Improved** ✅

- **Student 101 (Physics)**:
  - First score: **65**
  - Latest score: **60**
  - **Did not improve** ❌

- **Student 102 (Math)**:
  - First score: **80**
  - Latest score: **85**
  - **Improved** ✅

- **Student 103 (Math)**:
  - **Only one exam** → **Not eligible** ❌

- **Student 104 (Physics)**:
  - First score: **75**
  - Latest score: **85**
  - **Improved** ✅

**The output is ordered by `student_id` and `subject`.**

---

## Solution:

```sql
WITH RankedScores AS (
    SELECT 
        student_id, 
        subject, 
        score, 
        exam_date,
        RANK() OVER (PARTITION BY student_id, subject ORDER BY exam_date ASC) AS first_exam_rank,
        RANK() OVER (PARTITION BY student_id, subject ORDER BY exam_date DESC) AS latest_exam_rank
    FROM Scores
)

SELECT 
    r1.student_id, 
    r1.subject, 
    r1.score AS first_score, 
    r2.score AS latest_score
FROM RankedScores r1
JOIN RankedScores r2
ON r1.student_id = r2.student_id 
AND r1.subject = r2.subject
WHERE r1.first_exam_rank = 1
AND r2.latest_exam_rank = 1
AND r2.score > r1.score
ORDER BY r1.student_id, r1.subject;
