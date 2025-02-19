# Table: Trips

| Column Name | Type    |
|------------|--------|
| id         | int    |
| client_id  | int    |
| driver_id  | int    |
| city_id    | int    |
| status     | enum   |
| request_at | varchar|

**id** is the primary key (column with unique values) for this table.  
The table holds all taxi trips. Each trip has a unique id, while **client_id** and **driver_id** are foreign keys to the **users_id** at the Users table.  
**Status** is an ENUM type with values: ('completed', 'cancelled_by_driver', 'cancelled_by_client').  

# Table: Users

| Column Name | Type  |
|------------|------|
| users_id   | int  |
| banned     | enum |
| role       | enum |

**users_id** is the primary key (column with unique values) for this table.  
The table holds all users. Each user has a unique **users_id**, and **role** is an ENUM type with values: ('client', 'driver', 'partner').  
**banned** is an ENUM type with values: ('Yes', 'No').  

## Cancellation Rate Calculation
The cancellation rate is computed by dividing the number of canceled (by client or driver) requests with unbanned users by the total number of requests with unbanned users on that day.  

## Example
### Input
#### Trips Table
| id | client_id | driver_id | city_id | status              | request_at  |
|----|-----------|-----------|---------|---------------------|------------|
| 1  | 1         | 10        | 1       | completed           | 2013-10-01 |
| 2  | 2         | 11        | 1       | cancelled_by_driver | 2013-10-01 |
| 3  | 3         | 12        | 6       | completed           | 2013-10-01 |
| 4  | 4         | 13        | 6       | cancelled_by_client | 2013-10-01 |
| 5  | 1         | 10        | 1       | completed           | 2013-10-02 |
| 6  | 2         | 11        | 6       | completed           | 2013-10-02 |
| 7  | 3         | 12        | 6       | completed           | 2013-10-02 |
| 8  | 2         | 12        | 12      | completed           | 2013-10-03 |
| 9  | 3         | 10        | 12      | completed           | 2013-10-03 |
| 10 | 4         | 13        | 12      | cancelled_by_driver | 2013-10-03 |

#### Users Table
| users_id | banned | role   |
|----------|--------|--------|
| 1        | No     | client |
| 2        | Yes    | client |
| 3        | No     | client |
| 4        | No     | client |
| 10       | No     | driver |
| 11       | No     | driver |
| 12       | No     | driver |
| 13       | No     | driver |

### Output
| Day        | Cancellation Rate |
|------------|-------------------|
| 2013-10-01 | 0.33              |
| 2013-10-02 | 0.00              |
| 2013-10-03 | 0.50              |

### Explanation
- **2013-10-01**: 3 unbanned requests, 1 canceled → `1 / 3 = 0.33`
- **2013-10-02**: 2 unbanned requests, 0 canceled → `0 / 2 = 0.00`
- **2013-10-03**: 2 unbanned requests, 1 canceled → `1 / 2 = 0.50`

## SQL Query
```sql
SELECT 
    trips.request_at AS `Day`,
    ROUND(SUM(CASE WHEN trips.status IN ('cancelled_by_driver', 'cancelled_by_client') THEN 1 ELSE 0 END) / COUNT(*), 2) AS `Cancellation Rate`
FROM trips
JOIN users AS clients ON trips.client_id = clients.users_id AND clients.banned = 'No'
JOIN users AS drivers ON trips.driver_id = drivers.users_id AND drivers.banned = 'No'
WHERE trips.request_at BETWEEN '2013-10-01' AND '2013-10-03'
GROUP BY trips.request_at;
```