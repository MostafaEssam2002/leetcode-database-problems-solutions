# Rank Scores

## Problem Description

### Table: Scores

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| score       | decimal |

- `id` is the **primary key** (column with unique values) for this table.
- Each row of this table contains the **score of a game**.  
- `score` is a **floating point value** with **two decimal places**.

### Task:
Write a solution to **find the rank of the scores**.  
The ranking should be calculated according to the following rules:

1. The **scores should be ranked from highest to lowest**.
2. If there is a **tie** between two scores, **both should have the same ranking**.
3. After a tie, the next ranking number should be the **next consecutive integer value** (i.e., no holes between ranks).

Return the result table **ordered by score in descending order**.

### Example:

#### Example 1:

##### Input:
**Scores table:**

| id | score |
|----|-------|
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |

##### Output:

| score | rank |
|-------|------|
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |

#### Explanation:
- The **highest score is 4.00**, so **both occurrences rank as 1**.
- The **next highest score is 3.85**, so it gets **rank 2**.
- The **score 3.65 appears twice**, so both have **rank 3**.
- The **score 3.50 is the lowest**, so it gets **rank 4**.
- **Ranks are consecutive**, meaning there are **no missing numbers**.

---

## Solution:

```sql
SELECT score, 
       DENSE_RANK() OVER (ORDER BY score DESC) AS 'rank'
FROM Scores;
