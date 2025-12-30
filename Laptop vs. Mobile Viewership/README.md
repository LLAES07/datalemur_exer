This is the same question as problem #3 in the SQL Chapter of [Ace the Data Science Interview](https://amzn.to/3kF79Fx)!

Assume you're given the table on user viewership categorised by device type where the three types are laptop, tablet, and phone.

Write a query that calculates the total viewership for laptops and mobile devices where mobile is defined as the sum of tablet and phone viewership. Output the total viewership for laptops as `laptop_reviews` and the total viewership for mobile devices as `mobile_views`.

_Effective 15 April 2023, the solution has been updated with a more concise and easy-to-understand approach._

### `viewership` Table

|Column Name|Type|
|---|---|
|user_id|integer|
|device_type|string ('laptop', 'tablet', 'phone')|
|view_time|timestamp|


# Respuesta

```sql

SELECT 
    -- Calculamos el total de vistas por laptops. Cuando ve laptop deja un 1 y se suman terminando
  SUM(CASE 
    WHEN device_type = 'laptop' THEN 1 ELSE 0 END) AS laptop_views,

    -- Calculamos el total de vistas por tablet y telefono
  SUM(CASE 
    WHEN device_type IN ('phone', 'tablet') THEN 1 ELSE 0 END) AS mobile_views  

FROM viewership;


```

# Bonus

"Perfecto, ahora no me des los totales, dame el porcentaje de vistas que vinieron de dispositivos móviles frente al total general".

```sql

SELECT 
  ROUND(100.0 * SUM(CASE WHEN device_type = 'laptop' THEN 1 ELSE 0 END) / COUNT(*), 2) AS laptop_percent,
  ROUND(100.0 * SUM(CASE WHEN device_type IN ('tablet', 'phone') THEN 1 ELSE 0 END) / COUNT(*), 2) AS mobile_percent
FROM viewership;


```