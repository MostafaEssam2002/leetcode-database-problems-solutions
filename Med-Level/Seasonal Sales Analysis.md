\# 3564. Seasonal Sales Analysis



\*\*Difficulty:\*\* Medium

\*\*Status:\*\* Solved



\---



\## Tables



\### sales



| Column Name | Type    |

| ----------- | ------- |

| sale\_id     | int     |

| product\_id  | int     |

| sale\_date   | date    |

| quantity    | int     |

| price       | decimal |



\* `sale\_id` is the unique identifier for this table.

\* Each row contains information about a product sale including the `product\_id`, date of sale, quantity sold, and price per unit.



\### products



| Column Name  | Type    |

| ------------ | ------- |

| product\_id   | int     |

| product\_name | varchar |

| category     | varchar |



\* `product\_id` is the unique identifier for this table.

\* Each row contains information about a product including its name and category.



\---



\## Problem Description



Write a solution to find the \*\*most popular product category for each season\*\*.

The seasons are defined as:



\* Winter: December, January, February

\* Spring: March, April, May

\* Summer: June, July, August

\* Fall: September, October, November



The popularity of a category is determined by the \*\*total quantity sold\*\* in that season.

If there is a tie:



1\. Select the category with the \*\*highest total revenue\*\* (`quantity × price`).

2\. If there is still a tie, return the \*\*lexicographically smaller category\*\*.



Return the result table \*\*ordered by season in ascending order\*\*.



\---



\## Example



\### Input: sales table



| sale\_id | product\_id | sale\_date  | quantity | price |

| ------- | ---------- | ---------- | -------- | ----- |

| 1       | 1          | 2023-01-15 | 5        | 10.00 |

| 2       | 2          | 2023-01-20 | 4        | 15.00 |

| 3       | 3          | 2023-03-10 | 3        | 18.00 |

| 4       | 4          | 2023-04-05 | 1        | 20.00 |

| 5       | 1          | 2023-05-20 | 2        | 10.00 |

| 6       | 2          | 2023-06-12 | 4        | 15.00 |

| 7       | 5          | 2023-06-15 | 5        | 12.00 |

| 8       | 3          | 2023-07-24 | 2        | 18.00 |

| 9       | 4          | 2023-08-01 | 5        | 20.00 |

| 10      | 5          | 2023-09-03 | 3        | 12.00 |

| 11      | 1          | 2023-09-25 | 6        | 10.00 |

| 12      | 2          | 2023-11-10 | 4        | 15.00 |

| 13      | 3          | 2023-12-05 | 6        | 18.00 |

| 14      | 4          | 2023-12-22 | 3        | 20.00 |

| 15      | 5          | 2024-02-14 | 2        | 12.00 |



\### products table



| product\_id | product\_name   | category |

| ---------- | -------------- | -------- |

| 1          | Warm Jacket    | Apparel  |

| 2          | Designer Jeans | Apparel  |

| 3          | Cutting Board  | Kitchen  |

| 4          | Smart Speaker  | Tech     |

| 5          | Yoga Mat       | Fitness  |



\---



\### Output



| season | category | total\_quantity | total\_revenue |

| ------ | -------- | -------------- | ------------- |

| Fall   | Apparel  | 10             | 120.00        |

| Spring | Kitchen  | 3              | 54.00         |

| Summer | Tech     | 5              | 100.00        |

| Winter | Apparel  | 9              | 110.00        |



\---



\### Explanation



\* \*\*Fall (Sep, Oct, Nov):\*\*



&#x20; \* Apparel: 10 items sold (6 Jackets in Sep, 4 Jeans in Nov), revenue $120.00

&#x20; \* Fitness: 3 Yoga Mats sold, revenue $36.00

&#x20; \* Most popular: Apparel (highest total quantity)



\* \*\*Spring (Mar, Apr, May):\*\*



&#x20; \* Kitchen: 3 Cutting Boards, revenue $54.00

&#x20; \* Tech: 1 Smart Speaker, revenue $20.00

&#x20; \* Apparel: 2 Warm Jackets, revenue $20.00

&#x20; \* Most popular: Kitchen (highest quantity and revenue)



\* \*\*Summer (Jun, Jul, Aug):\*\*



&#x20; \* Tech: 5 Smart Speakers, revenue $100.00

&#x20; \* Fitness: 5 Yoga Mats, revenue $60.00

&#x20; \* Most popular: Tech (tie-breaker by revenue)



\* \*\*Winter (Dec, Jan, Feb):\*\*



&#x20; \* Apparel: 9 items, revenue $110.00

&#x20; \* Kitchen: 6 Cutting Boards, revenue $108.00

&#x20; \* Tech: 3 Smart Speakers, revenue $60.00

&#x20; \* Fitness: 2 Yoga Mats, revenue $24.00

&#x20; \* Most popular: Apparel (highest total quantity)



\---



\## Solution



```sql id="l2m7pt"

WITH cte AS (

&#x20;   SELECT 

&#x20;       sales.sale\_id,

&#x20;       sales.product\_id,

&#x20;       sales.quantity,

&#x20;       sales.price,

&#x20;       CASE 

&#x20;           WHEN MONTH(sales.sale\_date) IN (12,1,2) THEN 'Winter'

&#x20;           WHEN MONTH(sales.sale\_date) IN (3,4,5) THEN 'Spring'

&#x20;           WHEN MONTH(sales.sale\_date) IN (6,7,8) THEN 'Summer'

&#x20;           WHEN MONTH(sales.sale\_date) IN (9,10,11) THEN 'Fall'

&#x20;       END AS season,

&#x20;       sales.quantity \* sales.price AS revenue

&#x20;   FROM sales 

), agg AS (

&#x20;   SELECT 

&#x20;       cte.season,

&#x20;       products.category,

&#x20;       SUM(cte.quantity) AS total\_quantity,

&#x20;       SUM(cte.revenue) AS total\_revenue

&#x20;   FROM cte 

&#x20;   JOIN products  ON products.product\_id = cte.product\_id

&#x20;   GROUP BY cte.season, products.category

)

SELECT season, category, total\_quantity, total\_revenue

FROM (

&#x20;   SELECT \*,

&#x20;       ROW\_NUMBER() OVER(

&#x20;           PARTITION BY season 

&#x20;           ORDER BY total\_quantity DESC, total\_revenue DESC, category ASC

&#x20;       ) AS rn

&#x20;   FROM agg

) t

WHERE rn = 1

ORDER BY 

&#x20;   FIELD(season,'Fall','Spring','Summer','Winter');  -- ترتيب الفصول

```



