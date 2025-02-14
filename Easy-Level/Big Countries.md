# Big Countries Query

## Problem Description

The `World` table contains information about countries, including their name, continent, area, population, and GDP. The structure of the `World` table is as follows:

| Column Name | Type    |
|-------------|---------|
| name        | varchar |
| continent   | varchar |
| area        | int     |
| population  | int     |
| gdp         | bigint  |

- `name` is the primary key (column with unique values) for this table.
- Each row of this table gives information about the name of a country, the continent to which it belongs, its area, population, and GDP value.

A country is considered **big** if:

- It has an area of at least three million (3000000 km²), or
- It has a population of at least twenty-five million (25000000).

Your task is to find the name, population, and area of the big countries. 


## Solution

To find the big countries, we need to query the `World` table based on the conditions above. Here’s the SQL query that solves the problem:

```sql
SELECT name, population, area 
FROM World 
WHERE area >= 3000000 OR population >= 25000000;
