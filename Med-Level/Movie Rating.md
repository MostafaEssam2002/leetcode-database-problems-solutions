# Movie Rating Query Solution

## Tables Structure

### Movies Table

| Column Name | Type    |
|------------|--------|
| movie_id   | int    |
| title      | varchar |

- `movie_id` is the primary key.
- `title` represents the name of the movie.

### Users Table

| Column Name | Type    |
|------------|--------|
| user_id    | int    |
| name       | varchar |

- `user_id` is the primary key.
- `name` has unique values.

### MovieRating Table

| Column Name | Type    |
|------------|--------|
| movie_id   | int    |
| user_id    | int    |
| rating     | int    |
| created_at | date   |

- `(movie_id, user_id)` is the primary key.
- Stores movie ratings by users.
- `created_at` represents the date of the review.

## Problem Statement

Write a solution to:
1. Find the name of the user who has rated the most movies. In case of a tie, return the lexicographically smaller name.
2. Find the movie title with the highest average rating in February 2020. In case of a tie, return the lexicographically smaller title.

## Example

### Input:

#### Movies Table:

| movie_id | title   |
|---------|--------|
| 1       | Avengers |
| 2       | Frozen 2 |
| 3       | Joker   |

#### Users Table:

| user_id | name   |
|--------|-------|
| 1      | Daniel |
| 2      | Monica |
| 3      | Maria  |
| 4      | James  |

#### MovieRating Table:

| movie_id | user_id | rating | created_at |
|---------|--------|--------|------------|
| 1       | 1      | 3      | 2020-01-12  |
| 1       | 2      | 4      | 2020-02-11  |
| 1       | 3      | 2      | 2020-02-12  |
| 1       | 4      | 1      | 2020-01-01  |
| 2       | 1      | 5      | 2020-02-17  |
| 2       | 2      | 2      | 2020-02-01  |
| 2       | 3      | 2      | 2020-03-01  |
| 3       | 1      | 3      | 2020-02-22  |
| 3       | 2      | 4      | 2020-02-25  |

### Output:

| results  |
|---------|
| Daniel  |
| Frozen 2 |

### Explanation:
- Daniel and Monica have rated 3 movies, but "Daniel" is lexicographically smaller.
- "Frozen 2" and "Joker" have the highest average rating in February 2020, but "Frozen 2" is lexicographically smaller.

## SQL Solution

```sql
WITH UserRatings AS (
    SELECT u.name, COUNT(mr.movie_id) AS rating_count
    FROM MovieRating mr
    JOIN Users u ON mr.user_id = u.user_id
    GROUP BY u.name
),
MaxUser AS (
    SELECT name
    FROM UserRatings
    ORDER BY rating_count DESC, name ASC
    LIMIT 1
),
MovieAvgRating AS (
    SELECT m.title, 
           ROUND(AVG(mr.rating), 2) AS avg_rating
    FROM MovieRating mr
    JOIN Movies m ON mr.movie_id = m.movie_id
    WHERE mr.created_at BETWEEN '2020-02-01' AND '2020-02-29'
    GROUP BY m.title
),
MaxMovie AS (
    SELECT title
    FROM MovieAvgRating
    ORDER BY avg_rating DESC, title ASC
    LIMIT 1
)
SELECT name AS results FROM MaxUser
UNION ALL
SELECT title AS results FROM MaxMovie;
