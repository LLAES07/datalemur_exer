Given a table containing information about bank deposits and withdrawals made using Paypal, write a query to retrieve the final account balance for each account, taking into account all the transactions recorded in the table with the assumption that there are no missing transactions.

### `transactions` Table:

|**Column Name**|**Type**|
|---|---|
|transaction_id|integer|
|account_id|integer|
|amount|decimal|
|transaction_type|varchar|

### `transactions` Example Input:

|**transaction_id**|**account_id**|**amount**|**transaction_type**|
|---|---|---|---|
|123|101|10.00|Deposit|
|124|101|20.00|Deposit|
|125|101|5.00|Withdrawal|
|126|201|20.00|Deposit|
|128|201|10.00|Withdrawal|

# Respuesta

```sql
SELECT 
  account_id,
  -- Si está deposito lo suma y en caso que esté otra cosa lo resta (como solo tenemos dos posibilidades hacemos esto)
  SUM(CASE
    WHEN transaction_type = 'Deposit' THEN amount ELSE -amount END) AS balance
FROM transactions
GROUP BY
  account_id

```