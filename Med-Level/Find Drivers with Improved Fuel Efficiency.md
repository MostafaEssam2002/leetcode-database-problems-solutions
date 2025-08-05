# Find Drivers with Improved Fuel Efficiency

## Problem Description

Write a solution to find drivers whose fuel efficiency has improved by comparing their average fuel efficiency in the first half of the year with the second half of the year.

### Requirements

- Calculate fuel efficiency as `distance_km / fuel_consumed` for each trip
- **First half**: January to June (months 1-6)
- **Second half**: July to December (months 7-12)
- Only include drivers who have trips in **both halves** of the year
- Calculate the efficiency improvement as `(second_half_avg - first_half_avg)`
- Round all results to 2 decimal places
- Return the result table ordered by efficiency improvement in descending order, then by driver name in ascending order

## Database Schema

### Table: drivers

| Column Name | Type    |
|-------------|---------|
| driver_id   | int     |
| driver_name | varchar |

- `driver_id` is the unique identifier for this table
- Each row contains information about a driver

### Table: trips

| Column Name   | Type    |
|---------------|---------|
| trip_id       | int     |
| driver_id     | int     |
| trip_date     | date    |
| distance_km   | decimal |
| fuel_consumed | decimal |

- `trip_id` is the unique identifier for this table
- Each row represents a trip made by a driver, including the distance traveled and fuel consumed for that trip

## Sample Data

### drivers table:

| driver_id | driver_name   |
|-----------|---------------|
| 1         | Alice Johnson |
| 2         | Bob Smith     |
| 3         | Carol Davis   |
| 4         | David Wilson  |
| 5         | Emma Brown    |

### trips table:

| trip_id | driver_id | trip_date  | distance_km | fuel_consumed |
|---------|-----------|------------|-------------|---------------|
| 1       | 1         | 2023-02-15 | 120.5       | 10.2          |
| 2       | 1         | 2023-03-20 | 200.0       | 16.5          |
| 3       | 1         | 2023-08-10 | 150.0       | 11.0          |
| 4       | 1         | 2023-09-25 | 180.0       | 12.5          |
| 5       | 2         | 2023-01-10 | 100.0       | 9.0           |
| 6       | 2         | 2023-04-15 | 250.0       | 22.0          |
| 7       | 2         | 2023-10-05 | 200.0       | 15.0          |
| 8       | 3         | 2023-03-12 | 80.0        | 8.5           |
| 9       | 3         | 2023-05-18 | 90.0        | 9.2           |
| 10      | 4         | 2023-07-22 | 160.0       | 12.8          |
| 11      | 4         | 2023-11-30 | 140.0       | 11.0          |
| 12      | 5         | 2023-02-28 | 110.0       | 11.5          |

## Expected Output

| driver_id | driver_name   | first_half_avg | second_half_avg | efficiency_improvement |
|-----------|---------------|----------------|-----------------|------------------------|
| 2         | Bob Smith     | 11.24          | 13.33           | 2.10                   |
| 1         | Alice Johnson | 11.97          | 14.02           | 2.05                   |

## Detailed Explanation

### Alice Johnson (driver_id = 1):
- **First half trips (Jan-Jun)**: 
  - Feb 15: 120.5/10.2 = 11.81 km/L
  - Mar 20: 200.0/16.5 = 12.12 km/L
- **First half average efficiency**: (11.81 + 12.12) / 2 = 11.97 km/L
- **Second half trips (Jul-Dec)**:
  - Aug 10: 150.0/11.0 = 13.64 km/L
  - Sep 25: 180.0/12.5 = 14.40 km/L
- **Second half average efficiency**: (13.64 + 14.40) / 2 = 14.02 km/L
- **Efficiency improvement**: 14.02 - 11.97 = 2.05 km/L

### Bob Smith (driver_id = 2):
- **First half trips**: 
  - Jan 10: 100.0/9.0 = 11.11 km/L
  - Apr 15: 250.0/22.0 = 11.36 km/L
- **First half average efficiency**: (11.11 + 11.36) / 2 = 11.24 km/L
- **Second half trips**: 
  - Oct 5: 200.0/15.0 = 13.33 km/L
- **Second half average efficiency**: 13.33 km/L
- **Efficiency improvement**: 13.33 - 11.24 = 2.10 km/L (rounded to 2 decimal places)

### Drivers not included:
- **Carol Davis (driver_id = 3)**: Only has trips in first half (Mar, May)
- **David Wilson (driver_id = 4)**: Only has trips in second half (Jul, Nov)
- **Emma Brown (driver_id = 5)**: Only has trips in first half (Feb)

The output table is ordered by efficiency improvement in descending order, then by driver name in ascending order.

## SQL Solution

```sql
WITH final AS (
    SELECT 
        drivers.driver_id,
        drivers.driver_name,
        AVG(
            CASE WHEN MONTH(trips.trip_date) <= 6 
                 THEN trips.distance_km / trips.fuel_consumed 
                 ELSE NULL 
            END
        ) AS first_half_avg,
        AVG(
            CASE WHEN MONTH(trips.trip_date) > 6 
                 THEN trips.distance_km / trips.fuel_consumed 
                 ELSE NULL 
            END
        ) AS second_half_avg 
    FROM trips 
    JOIN drivers ON drivers.driver_id = trips.driver_id  
    GROUP BY drivers.driver_id, drivers.driver_name
    HAVING 
        -- Ensure driver has trips in both halves
        SUM(CASE WHEN MONTH(trips.trip_date) <= 6 THEN 1 ELSE 0 END) > 0 
        AND SUM(CASE WHEN MONTH(trips.trip_date) > 6 THEN 1 ELSE 0 END) > 0
)

SELECT 
    driver_id,
    driver_name,
    ROUND(first_half_avg, 2) AS first_half_avg,
    ROUND(second_half_avg, 2) AS second_half_avg,
    ROUND(second_half_avg - first_half_avg, 2) AS efficiency_improvement  
FROM final 
WHERE second_half_avg > first_half_avg  -- Only drivers with improvement
ORDER BY efficiency_improvement DESC, driver_name ASC;
```

## Alternative Solution (Original Approach)

```sql
WITH final AS (
    SELECT 
        drivers.driver_id,
        drivers.driver_name,
        AVG(
            (CASE WHEN MONTH(trips.trip_date) <= 6 THEN trips.distance_km ELSE 0 END)
            / (CASE WHEN MONTH(trips.trip_date) <= 6 THEN trips.fuel_consumed ELSE 0 END)
        ) AS first_half_avg,
        AVG(
            (CASE WHEN MONTH(trips.trip_date) > 6 THEN trips.distance_km ELSE 0 END) / 
            (CASE WHEN MONTH(trips.trip_date) > 6 THEN trips.fuel_consumed ELSE 0 END)
        ) AS second_half_avg 
    FROM trips 
    JOIN drivers ON drivers.driver_id = trips.driver_id  
    GROUP BY drivers.driver_id 
    HAVING 
        SUM(CASE WHEN MONTH(trips.trip_date) <= 6 THEN trips.fuel_consumed ELSE 0 END) > 0 
        AND SUM(CASE WHEN MONTH(trips.trip_date) > 6 THEN trips.fuel_consumed ELSE 0 END) > 0
)

SELECT 
    driver_id,
    driver_name,
    ROUND(first_half_avg, 2) AS first_half_avg,
    ROUND(second_half_avg, 2) AS second_half_avg,
    ROUND(second_half_avg - first_half_avg, 2) AS efficiency_improvement  
FROM final 
HAVING efficiency_improvement > 0  
ORDER BY efficiency_improvement DESC, driver_name ASC;
```

## Solution Breakdown

1. **CTE (Common Table Expression) - `final`**:
   - Joins `trips` and `drivers` tables
   - Calculates fuel efficiency for each trip as `distance_km / fuel_consumed`
   - Separates trips into first half (months 1-6) and second half (months 7-12)
   - Calculates average efficiency for each half using conditional aggregation
   - Filters drivers who have trips in both halves using `HAVING` clause

2. **Main Query**:
   - Rounds all efficiency values to 2 decimal places
   - Calculates efficiency improvement as the difference between second and first half
   - Filters only drivers with positive improvement
   - Orders results by improvement (descending) and driver name (ascending)

## Key SQL Functions Used

- `MONTH(date)`: Extracts month number from date
- `CASE WHEN`: Conditional logic for separating halves
- `AVG()`: Calculates average efficiency
- `ROUND(value, 2)`: Rounds to 2 decimal places
- `HAVING`: Filters grouped results
- `JOIN`: Combines driver and trip data

## Performance Considerations

- Index on `trip_date` and `driver_id` columns would improve performance
- The solution handles edge cases like division by zero through proper filtering
- Using `NULL` in CASE statements (first solution) is cleaner than using 0 (second solution)
