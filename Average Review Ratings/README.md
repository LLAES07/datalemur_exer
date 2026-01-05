Given the reviews table, write a query to retrieve the average star rating for each product, grouped by month. The output should display the month as a numerical value, product ID, and average star rating rounded to two decimal places. Sort the output first by month and then by product ID.

P.S. If you've read the [Ace the Data Science Interview](https://amzn.to/3kF79Fx), and liked it, consider writing us a review?

### `reviews` Table:

|**Column Name**|**Type**|
|---|---|
|review_id|integer|
|user_id|integer|
|submit_date|datetime|
|product_id|integer|
|stars|integer (1-5)|

### `reviews` Example Input:

|**review_id**|**user_id**|**submit_date**|**product_id**|**stars**|
|---|---|---|---|---|
|6171|123|06/08/2022 00:00:00|50001|4|
|7802|265|06/10/2022 00:00:00|69852|4|
|5293|362|06/18/2022 00:00:00|50001|3|
|6352|192|07/26/2022 00:00:00|69852|3|
|4517|981|07/05/2022 00:00:00|69852|2|


# Respuesta

```sql

-- AVG START RATING POR CADA PRODUCTO AGRUPADO POR MES
-- MONTH AS NUMERICAL, PRODUCT ID, AVERAGE REDONDEADO A 2 DECIMALES
-- SORT MONTH AND THEN PRODUCT

SELECT
    -- Extrae los meses de forma numerica
  EXTRACT(MONTH FROM submit_date ) as meses,
  product_id,
    -- Genera la agrupación para el promedio
  ROUND(AVG(stars), 2) as avg_stars
FROM reviews
GROUP BY 
    -- Agrupa primero por mes y después por producto
  EXTRACT(MONTH FROM submit_date ),
  product_id
ORDER BY 
    -- Ordena primero por mes y después por producto
  meses,
  product_id;


```