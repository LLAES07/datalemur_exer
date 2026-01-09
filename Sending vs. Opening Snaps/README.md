This is the same question as problem #25 in the SQL Chapter of [Ace the Data Science Interview](https://amzn.to/3kF79Fx)!

Assume you're given tables with information on Snapchat users, including their ages and time spent sending and opening snaps.

Write a query to obtain a breakdown of the time spent sending vs. opening snaps as a percentage of total time spent on these activities grouped by age group. Round the percentage to 2 decimal places in the output.

Notes:

- Calculate the following percentages:
    - time spent sending / (Time spent sending + Time spent opening)
    - Time spent opening / (Time spent sending + Time spent opening)
- To avoid integer division in percentages, multiply by 100.0 and not 100.

_Effective April 15th, 2023, the solution has been updated and optimised._

### `activities` Table

|Column Name|Type|
|---|---|
|activity_id|integer|
|user_id|integer|
|activity_type|string ('send', 'open', 'chat')|
|time_spent|float|
|activity_date|datetime|

### `activities` Example Input

|activity_id|user_id|activity_type|time_spent|activity_date|
|---|---|---|---|---|
|7274|123|open|4.50|06/22/2022 12:00:00|
|2425|123|send|3.50|06/22/2022 12:00:00|
|1413|456|send|5.67|06/23/2022 12:00:00|
|1414|789|chat|11.00|06/25/2022 12:00:00|
|2536|456|open|3.00|06/25/2022 12:00:00|

### `age_breakdown` Table

|Column Name|Type|
|---|---|
|user_id|integer|
|age_bucket|string ('21-25', '26-30', '31-25')|

### `age_breakdown` Example Input

|user_id|age_bucket|
|---|---|
|123|31-35|
|456|26-30|
|789|21-25|


````sql
WITH totals AS (
    SELECT
        b.age_bucket,
        SUM(CASE WHEN activity_type = 'open' THEN time_spent ELSE 0 END) +
        SUM(CASE WHEN activity_type = 'send' THEN time_spent ELSE 0 END) AS opening_time
    FROM activities a
    INNER JOIN age_breakdown b ON a.user_id = b.user_id
    GROUP BY b.age_bucket
)
SELECT
    b.age_bucket,
    ROUND(100.0 * SUM(CASE WHEN activity_type = 'send' THEN time_spent ELSE 0 END) / t.opening_time, 2) AS send_percentage,
    ROUND(100.0 * SUM(CASE WHEN activity_type = 'open' THEN time_spent ELSE 0 END) / t.opening_time, 2) AS open_percentage
FROM activities a
INNER JOIN age_breakdown b ON a.user_id = b.user_id
INNER JOIN totals t ON t.age_bucket = b.age_bucket
GROUP BY b.age_bucket, t.opening_time
ORDER BY age_bucket



```