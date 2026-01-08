his is the same question as problem #11 in the SQL Chapter of [Ace the Data Science Interview](https://amzn.to/3kF79Fx)!

Assume you are given the table below on Uber transactions made by users. Write a query to obtain the third transaction of every user. Output the user id, spend and transaction date.

### `transactions` Table:

|**Column Name**|**Type**|
|---|---|
|user_id|integer|
|spend|decimal|
|transaction_date|timestamp|

### `transactions` Example Input:

| **user_id** | **spend** | **transaction_date** |
| ----------- | --------- | -------------------- |
| 111         | 100.50    | 01/08/2022 12:00:00  |
| 111         | 55.00     | 01/10/2022 12:00:00  |
| 121         | 36.00     | 01/18/2022 12:00:00  |
| 145         | 24.99     | 01/26/2022 12:00:00  |
| 111         | 89.60     | 02/05/2022 12:00:00  |

# Respuesta

```sql

WITH t1 AS (
    -- Tabla para general el ranking
    SELECT
    *,
    -- Genera un raking ordenado según la id y fecha de transacción
    ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY transaction_date ASC) AS ranking
    FROM transactions 
)

SELECT
    -- Consulta final donde solo se escogen la tercera transacción
  user_id,
  spend,
  transaction_date
FROM
  t1
WHERE
    -- filtra solo la tercera transacción de cada usuario
  ranking =3



```
