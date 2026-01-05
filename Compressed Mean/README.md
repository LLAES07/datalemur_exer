
You're trying to find the mean number of items per order on Alibaba, rounded to 1 decimal place using tables which includes information on the count of items in each order (`item_count` table) and the corresponding number of orders for each item count (`order_occurrences` table).

### `items_per_order` Table:

|Column Name|Type|
|---|---|
|item_count|integer|
|order_occurrences|integer|

### `items_per_order` Example Input:

|item_count|order_occurrences|
|---|---|
|1|500|
|2|1000|
|3|800|
|4|1000|

There are a total of 500 orders with one item per order, 1000 orders with two items per order, and 800 orders with three items per order."

# Respuesta

```sql


WITH total_orders AS (

  SELECT
    -- Ordenes totales
    SUM(order_occurrences) AS total_orders

  FROM
    items_per_order

)

SELECT
    -- multiplica la cantidad de ordenes y la cantidad de items de cada una para sumarse y dividirse por el total de ordenes para sacar el promedio
  ROUND((SUM(order_occurrences*item_count) / (SELECT *FROM total_orders))::numeric, 1)
  
FROM
  items_per_order

```