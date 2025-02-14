# Problem Description: Average Time to Complete a Process per Machine

In a factory website, several machines are running processes. Each process is marked with two activities: 
- **start**: When the machine starts the process.
- **end**: When the machine finishes the process.

The goal is to compute the **average time** each machine takes to complete a process, where the time to complete a process is the difference between the `end` timestamp and the `start` timestamp for each process.

## Input Table: `Activity`

The `Activity` table has the following columns:

| Column Name   | Type    | Description                                                                                                                                              |
|---------------|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| machine_id    | int     | The ID of the machine.                                                                                                                                     |
| process_id    | int     | The ID of the process running on the machine.                                                                                                              |
| activity_type | enum    | The type of activity: either `'start'` or `'end'`.                                                                                                        |
| timestamp     | float   | The timestamp in seconds when the activity occurred. The `start` timestamp is guaranteed to be before the `end` timestamp for each (machine_id, process_id) pair. |

### Primary Key: (machine_id, process_id)

For each `(machine_id, process_id)` pair, there will always be a corresponding `'start'` and `'end'` timestamp.

## Goal

We need to calculate the **average time** it takes each machine to complete a process. The time for a process is calculated as the difference between the `end` timestamp and the `start` timestamp. The final result should include:
- `machine_id`: The ID of the machine.
- `processing_time`: The average processing time, rounded to 3 decimal places.

### Expected Output

The output should show:
- The `machine_id`.
- The calculated **average processing time** for each machine.

## Example

### Input

| machine_id | process_id | activity_type | timestamp |
|------------|------------|---------------|-----------|
| 0          | 0          | start         | 0.712     |
| 0          | 0          | end           | 1.520     |
| 0          | 1          | start         | 3.140     |
| 0          | 1          | end           | 4.120     |
| 1          | 0          | start         | 0.550     |
| 1          | 0          | end           | 1.550     |
| 1          | 1          | start         | 0.430     |
| 1          | 1          | end           | 1.420     |
| 2          | 0          | start         | 4.100     |
| 2          | 0          | end           | 4.512     |
| 2          | 1          | start         | 2.500     |
| 2          | 1          | end           | 5.000     |

### Output

| machine_id | processing_time |
|------------|-----------------|
| 0          | 0.894           |
| 1          | 0.995           |
| 2          | 1.456           |

### Explanation

1. **Machine 0**:
   - Process 0: Time = 1.520 - 0.712 = 0.808
   - Process 1: Time = 4.120 - 3.140 = 0.980
   - Average time = (0.808 + 0.980) / 2 = 0.894

2. **Machine 1**:
   - Process 0: Time = 1.550 - 0.550 = 1.000
   - Process 1: Time = 1.420 - 0.430 = 0.990
   - Average time = (1.000 + 0.990) / 2 = 0.995

3. **Machine 2**:
   - Process 0: Time = 4.512 - 4.100 = 0.412
   - Process 1: Time = 5.000 - 2.500 = 2.500
   - Average time = (0.412 + 2.500) / 2 = 1.456

## SQL Solution

```sql
SELECT 
    machine_id, 
    ROUND(AVG(end_time - start_time), 3) AS processing_time
FROM (
    SELECT 
        machine_id,
        process_id,
        CASE WHEN activity_type = 'start' THEN timestamp END AS start_time,
        MAX(CASE WHEN activity_type = 'end' THEN timestamp END) AS end_time
    FROM Activity
    GROUP BY machine_id, process_id
) AS s
GROUP BY machine_id;
