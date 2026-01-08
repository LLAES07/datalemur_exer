This is the same question as problem #1 in the SQL Chapter of [Ace the Data Science Interview](https://amzn.to/3kF79Fx)!

Assume you have an events table on Facebook app analytics. Write a query to calculate the click-through rate (CTR) for the app in 2022 and round the results to 2 decimal places.

Definition and note:

- Percentage of click-through rate (CTR) = 100.0 * Number of clicks / Number of impressions
- To avoid integer division, multiply the CTR by 100.0, not 100.

### `events` Table:

|Column Name|Type|
|---|---|
|app_id|integer|
|event_type|string|
|timestamp|datetime|

### `events` Example Input:

| app_id | event_type | timestamp           |
| ------ | ---------- | ------------------- |
| 123    | impression | 07/18/2022 11:36:12 |
| 123    | impression | 07/18/2022 11:37:12 |
| 123    | click      | 07/18/2022 11:37:42 |
| 234    | impression | 07/18/2022 14:15:12 |
| 234    | click      | 07/18/2022 14:16:12 |


# Respuesta


```sql

SELECT
  app_id,
  ROUND(100.0 *
    -- Cuando el evento es click 1 y cuando es impression 1 y se cuenta cuantos hay
    COUNT(CASE WHEN event_type = 'click' THEN 1 ELSE NULL END) /
    COUNT(CASE WHEN event_type = 'impression' THEN 1 ELSE NULL END), 2)  AS ctr_rate
FROM events
WHERE
  EXTRACT(YEAR FROM timestamp) ='2022'
GROUP BY app_id;

```