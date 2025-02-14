## Problem Description

### Table: Tweets

| Column Name | Type    |
|-------------|---------|
| tweet_id    | int     |
| content     | varchar |

- `tweet_id` is the primary key (column with unique values) for this table.
- `content` consists of characters on an American Keyboard, and no other special characters.
- This table contains all the tweets in a social media app.

### Task:
Write a query to find the IDs of the invalid tweets. A tweet is considered invalid if the number of characters in the content of the tweet is strictly greater than 15.

### Example:

#### Input:
Tweets table:

| tweet_id | content                           |
|----------|-----------------------------------|
| 1        | Let us Code                       |
| 2        | More than fifteen chars are here! |

#### Output:

| tweet_id |
|----------|
| 2        |

#### Explanation:
- Tweet 1 has a length of 11. It is a valid tweet.
- Tweet 2 has a length of 33. It is an invalid tweet.

---

## Solution:

```sql
SELECT tweet_id
FROM Tweets
WHERE LENGTH(content) > 15;
