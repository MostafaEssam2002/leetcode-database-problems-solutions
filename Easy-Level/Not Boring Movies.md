# SQL Query: Movies with Odd IDs and Non-Boring Descriptions

## Problem Statement
You are given a table named `Cinema` with the following structure:

| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| movie         | varchar |
| description   | varchar |
| rating        | float   |

- `id` is the primary key for this table.
- Each row contains information about a movie's name, its description, and its rating.
- The `rating` column is a float value in the range `[0, 10]` with two decimal places.

### Objective
Write a query to:
1. Select movies with an **odd-numbered `id`**.
2. Exclude movies where the `description` is **"boring"**.
3. Sort the result table by **rating in descending order**.

Return the result in the following format:

| id  | movie      | description | rating |
|-----|------------|-------------|--------|
| 5   | House card | Interesting | 9.1    |
| 1   | War        | great 3D    | 8.9    |

---

## Solution

```sql
SELECT id, movie, description, rating
FROM Cinema
WHERE id % 2 != 0 AND description != 'boring'
ORDER BY rating DESC;
