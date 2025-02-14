# SQL Challenge: Last Person to Board the Bus

## Table: Queue

| Column Name | Type    |
|-------------|---------|
| person_id   | int     |
| person_name | varchar |
| weight      | int     |
| turn        | int     |

- `person_id` column contains unique values.
- This table has the information about all people waiting for a bus.
- The `person_id` and `turn` columns will contain all numbers from `1` to `n`, where `n` is the number of rows in the table.
- `turn` determines the order in which the people will board the bus, where `turn=1` denotes the first person to board and `turn=n` denotes the last person to board.
- `weight` is the weight of the person in kilograms.

## Problem Statement

There is a queue of people waiting to board a bus. However, the bus has a **weight limit of 1000 kilograms**, so some people may not be able to board.

Write a solution to find the **person_name** of the last person who can fit on the bus **without exceeding the weight limit**. The test cases ensure that the first person does not exceed the weight limit.

**Note:** Only one person can board the bus at a given turn.

## Example

### **Input**: 

Queue table:

| person_id | person_name | weight | turn |
|-----------|-------------|--------|------|
| 5         | Alice       | 250    | 1    |
| 4         | Bob         | 175    | 5    |
| 3         | Alex        | 350    | 2    |
| 6         | John Cena   | 400    | 3    |
| 1         | Winston     | 500    | 6    |
| 2         | Marie       | 200    | 4    |

### **Output**:

| person_name |
|-------------|
| John Cena   |

### **Explanation**:

Below is the ordered table based on `turn`:

| Turn | ID | Name      | Weight | Total Weight |
|------|----|-----------|--------|--------------|
| 1    | 5  | Alice     | 250    | 250          |
| 2    | 3  | Alex      | 350    | 600          |
| 3    | 6  | John Cena | 400    | 1000         | (Last person to board) |
| 4    | 2  | Marie     | 200    | 1200         | (Cannot board) |
| 5    | 4  | Bob       | 175    | ___          |
| 6    | 1  | Winston   | 500    | ___          |

## SQL Solution

```sql
WITH BoardingQueue AS (
    SELECT person_name, weight, turn,
           SUM(weight) OVER (ORDER BY turn) AS total_weight
    FROM Queue
)
SELECT person_name 
FROM BoardingQueue
WHERE total_weight <= 1000
ORDER BY total_weight DESC
LIMIT 1;
