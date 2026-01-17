Sometimes, payment transactions are repeated by accident; it could be due to user error, API failure or a retry error that causes a credit card to be charged twice.

Using the transactions table, identify any payments made at the same merchant with the same credit card for the same amount within 10 minutes of each other. Count such repeated payments.

**Assumptions:**

- The first transaction of such payments should not be counted as a repeated payment. This means, if there are two transactions performed by a merchant with the same credit card and for the same amount within 10 minutes, there will only be 1 repeated payment.

### `transactions` Table:

|**Column Name**|**Type**|
|---|---|
|transaction_id|integer|
|merchant_id|integer|
|credit_card_id|integer|
|amount|integer|
|transaction_timestamp|datetime|

### `transactions` Example Input:

|**transaction_id**|**merchant_id**|**credit_card_id**|**amount**|**transaction_timestamp**|
|---|---|---|---|---|
|1|101|1|100|09/25/2022 12:00:00|
|2|101|1|100|09/25/2022 12:08:00|
|3|101|1|100|09/25/2022 12:28:00|
|4|102|2|300|09/25/2022 12:00:00|
|6|102|2|400|09/25/2022 14:00:00|

# Respuesta

```sql
WITH ordered AS (
  SELECT
    transaction_id,
    merchant_id,
    credit_card_id,
    amount,
    transaction_timestamp,
    -- Lag para poder restar la transacción actual con la anterior (pone el valor de la fila 1 en la columna nueva pero en la fila 2)
    LAG(transaction_timestamp) OVER ( PARTITION BY merchant_id, credit_card_id, amount ORDER BY transaction_timestamp) AS prev_ts
  FROM 
    transactions
)

-- Cuenta el total de pagos repetidos
SELECT COUNT(*) AS repeated_payments
FROM ordered
-- No puede ser nulo y la resta debe ser menor o igual a 10 min
WHERE prev_ts IS NOT NULL
  AND transaction_timestamp - prev_ts <= INTERVAL '10 minutes';



 ```