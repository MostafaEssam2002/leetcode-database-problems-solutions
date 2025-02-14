# Problem Description

You are given a table called `Queries` that contains information about various database queries.

The table consists of the following columns:
- `query_name`: The name of the query.
- `result`: The result of the query.
- `position`: The position of the query (ranging from 1 to 500).
- `rating`: The rating of the query (ranging from 1 to 5).

Your task is to:
1. Calculate the **Query Quality** for each `query_name`, which is defined as the average of the ratio between the query's rating and its position.
2. Calculate the **Poor Query Percentage** for each `query_name`, which is the percentage of queries with a rating less than 3.

Both **Query Quality** and **Poor Query Percentage** should be rounded to 2 decimal places.

---

## Example Input

### `Queries` Table:

| query_name | result            | position | rating |
|------------|-------------------|----------|--------|
| Dog        | Golden Retriever  | 1        | 5      |
| Dog        | German Shepherd   | 2        | 5      |
| Dog        | Mule              | 200      | 1      |
| Cat        | Shirazi           | 5        | 2      |
| Cat        | Siamese           | 3        | 3      |
| Cat        | Sphynx            | 7        | 4      |

---

## Example Output

| query_name | quality | poor_query_percentage |
|------------|---------|-----------------------|
| Dog        | 2.50    | 33.33                 |
| Cat        | 0.66    | 33.33                 |

---

## Explanation

1. **Dog**:
    - Query Quality:  
      ```
      quality = ((5 / 1) + (5 / 2) + (1 / 200)) / 3 = 2.50
      ```
    - Poor Query Percentage:  
      ```
      poor_query_percentage = (1 / 3) * 100 = 33.33
      ```

2. **Cat**:
    - Query Quality:  
      ```
      quality = ((2 / 5) + (3 / 3) + (4 / 7)) / 3 = 0.66
      ```
    - Poor Query Percentage:  
      ```
      poor_query_percentage = (1 / 3) * 100 = 33.33
      ```

---

# Solution

### SQL Query

```sql
SELECT  
    query_name,
    ROUND(SUM(rating / position) / COUNT(query_name), 2) AS quality, 
    ROUND((COUNT(CASE WHEN rating < 3 THEN 1 END) / COUNT(query_name)) * 100, 2) AS poor_query_percentage 
FROM queries 
GROUP BY query_name;
