# Finding Number of Different Products Sold Per Day

The task requires finding for each date the number of different products sold and their names. The sold products' names for each date should be sorted lexicographically. The result should be ordered by `sell_date`.

## Table Schema
### Input Table: `Activities`
| Column Name | Type    |
|-------------|---------|
| sell_date   | date    |
| product     | varchar |

- The `sell_date` column contains the date when a product was sold.
- The `product` column contains the name of the product sold.
- There is no primary key (column with unique values) for this table. It may contain duplicates.

### Example Input
#### Activities Table:
| sell_date | product     |
|-----------|-------------|
| 2020-05-30 | Headphone  |
| 2020-06-01 | Pencil     |
| 2020-06-02 | Mask       |
| 2020-05-30 | Basketball |
| 2020-06-01 | Bible      |
| 2020-06-02 | Mask       |
| 2020-05-30 | T-Shirt    |

### Expected Output
| sell_date  | num_sold | products                     |
|------------|----------|------------------------------|
| 2020-05-30 | 3        | Basketball,Headphone,T-shirt |
| 2020-06-01 | 2        | Bible,Pencil                 |
| 2020-06-02 | 1        | Mask                         |

### Explanation:
- For `2020-05-30`, the sold items were (Headphone, Basketball, T-shirt). They are sorted lexicographically and separated by a comma.
- For `2020-06-01`, the sold items were (Pencil, Bible). They are sorted lexicographically and separated by a comma.
- For `2020-06-02`, the sold item is (Mask), so it is just returned.

## SQL Query Solution
The following SQL query achieves the desired result by:
1. Using `COUNT(DISTINCT product)` to count the number of distinct products sold for each date.
2. Using `GROUP_CONCAT(DISTINCT product ORDER BY product)` to concatenate the distinct products sorted lexicographically.
3. Grouping by `sell_date` to get the results for each date.

### SQL Query
```sql
SELECT 
    sell_date,
    COUNT(distinct product) AS num_sold,
    GROUP_CONCAT(DISTINCT product ORDER BY product) AS products
FROM activities
GROUP BY sell_date;
