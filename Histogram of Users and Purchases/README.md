This is the same question as problem #13 in the SQL Chapter of [Ace the Data Science Interview](https://amzn.to/3kF79Fx)!

Assume you're given a table on Walmart user transactions. Based on their most recent transaction date, write a query that retrieve the users along with the number of products they bought.

Output the user's most recent transaction date, user ID, and the number of products, sorted in chronological order by the transaction date.

### `user_transactions` Table:

|Column Name|Type|
|---|---|
|product_id|integer|
|user_id|integer|
|spend|decimal|
|transaction_date|timestamp|

### `user_transactions` Example Input:

| product_id | user_id | spend  | transaction_date    |
| ---------- | ------- | ------ | ------------------- |
| 3673       | 123     | 68.90  | 07/08/2022 12:00:00 |
| 9623       | 123     | 274.10 | 07/08/2022 12:00:00 |
| 1467       | 115     | 19.90  | 07/08/2022 12:00:00 |
| 2513       | 159     | 25.00  | 07/08/2022 12:00:00 |
| 1452       | 159     | 74.50  | 07/10/2022 12:00:00 |


# Respuesta

```sql
WITH ranking AS (
  SELECT
    *,
    DENSE_RANK() OVER(PARTITION BY user_id ORDER BY transaction_date DESC) AS rk
  
  FROM
    user_transactions
)

SELECT
  transaction_date,
  user_id,
  COUNT(*) AS purchase_count
FROM
  ranking
WHERE
  rk = 1
GROUP BY  
  transaction_date,
  user_id

```