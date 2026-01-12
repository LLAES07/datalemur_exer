In an effort to identify high-value customers, Amazon asked for your help to obtain data about users who go on shopping sprees. A shopping spree occurs when a user makes purchases on 3 or more consecutive days.

List the user IDs who have gone on at least 1 shopping spree in ascending order.

### `transactions` Table:

|**Column Name**|**Type**|
|---|---|
|user_id|integer|
|amount|float|
|transaction_date|timestamp|

### `transactions` Example Input:

| user_id | amount | transaction_date    |
| ------- | ------ | ------------------- |
| 1       | 9.99   | 08/01/2022 10:00:00 |
| 1       | 55     | 08/17/2022 10:00:00 |
| 2       | 149.5  | 08/05/2022 10:00:00 |
| 2       | 4.89   | 08/06/2022 10:00:00 |
| 2       | 34     | 08/07/2022 10:00:00 |


# Respuesta

```sql
WITH fechas_unicas AS (
    --solo días únicos por usuario
    SELECT DISTINCT user_id, CAST(transaction_date AS DATE) AS t_date
    FROM transactions
),
ct1 AS (
    SELECT
      user_id,
      t_date,
      -- El día siguiente
      LEAD(t_date, 1) OVER (PARTITION BY user_id ORDER BY t_date) AS dia_2,
      -- dos dias después
      LEAD(t_date, 2) OVER (PARTITION BY user_id ORDER BY t_date) AS dia_3
    
    FROM
      fechas_unicas )

-- Consulta final
SELECT
  DISTINCT 
    user_id
FROM
  ct1
WHERE
  dia_2 = t_date + INTERVAL '1 day' AND
  dia_3 = t_date + INTERVAL '2 days'

  
  

```