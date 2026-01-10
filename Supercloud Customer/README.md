A Microsoft Azure Supercloud customer is defined as a customer who has purchased at least one product from every product category listed in the `products` table.

Write a query that identifies the customer IDs of these Supercloud customers.

### `customer_contracts` Table:

|Column Name|Type|
|---|---|
|customer_id|integer|
|product_id|integer|
|amount|integer|

### `customer_contracts` Example Input:

|customer_id|product_id|amount|
|---|---|---|
|1|1|1000|
|1|3|2000|
|1|5|1500|
|2|2|3000|
|2|6|2000|

### `products` Table:

|Column Name|Type|
|---|---|
|product_id|integer|
|product_category|string|
|product_name|string|

### `products` Example Input:

| product_id | product_category | product_name             |
| ---------- | ---------------- | ------------------------ |
| 1          | Analytics        | Azure Databricks         |
| 2          | Analytics        | Azure Stream Analytics   |
| 4          | Containers       | Azure Kubernetes Service |
| 5          | Containers       | Azure Service Fabric     |
| 6          | Compute          | Virtual Machines         |
| 7          | Compute          | Azure Functions          |


# Respuesta

```sql


SELECT
    c.customer_id

FROM
    customer_contracts c
    INNER JOIN products p ON c.product_id = p.product_id
GROUP BY
    c.customer_id
    -- Aqui utilizamos Having con un count para cuando se agrupe cuente para cada usuario cuantas categorias distintas tiene y si son iguales a la cantidad total de categorias entonces es un Supercloud y devolvemos su id.
HAVING COUNT(DISTINCT p.product_category ) = (SELECT COUNT(DISTINCT product_category) FROM products)



```