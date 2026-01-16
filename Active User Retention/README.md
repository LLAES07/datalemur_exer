This is the same question as problem #23 in the SQL Chapter of [Ace the Data Science Interview](https://amzn.to/3kF79Fx)!

Assume you're given a table containing information on Facebook user actions. Write a query to obtain number of monthly active users (MAUs) in July 2022, including the month in numerical format "1, 2, 3".

Hint:

- An active user is defined as a user who has performed actions such as 'sign-in', 'like', or 'comment' in both the current month and the previous month.

### `user_actions` Table:

|Column Name|Type|
|---|---|
|user_id|integer|
|event_id|integer|
|event_type|string ("sign-in, "like", "comment")|
|event_date|datetime|

### `user_actions`Example Input:

|user_id|event_id|event_type|event_date|
|---|---|---|---|
|445|7765|sign-in|05/31/2022 12:00:00|
|742|6458|sign-in|06/03/2022 12:00:00|
|445|3634|like|06/05/2022 12:00:00|
|742|1374|comment|06/05/2022 12:00:00|
|648|3124|like|06/18/2022 12:00:00|


# Respuesta

```sql
WITH m AS (
  SELECT
    user_id,
    DATE_TRUNC('month', event_date) AS month_start
  FROM user_actions
  WHERE event_type IN ('sign-in', 'like', 'comment')
  GROUP BY user_id, DATE_TRUNC('month', event_date)
),
active_in_july AS (
  SELECT user_id
  FROM m
  WHERE month_start IN (DATE '2022-06-01', DATE '2022-07-01')
  GROUP BY user_id
  HAVING COUNT(*) = 2
)
SELECT
  7 AS month,
  COUNT(*) AS monthly_active_users
FROM active_in_july;


```