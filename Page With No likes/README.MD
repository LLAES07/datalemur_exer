Assume you're given two tables containing data about Facebook Pages and their respective likes (as in "Like a Facebook Page").

Write a query to return the IDs of the Facebook pages that have zero likes. The output should be sorted in ascending order based on the page IDs.

### `pages` Table:

|**Column Name**|**Type**|
|---|---|
|page_id|integer|
|page_name|varchar|

### `pages` Example Input:

|**page_id**|**page_name**|
|---|---|
|20001|SQL Solutions|
|20045|Brain Exercises|
|20701|Tips for Data Analysts|

### `page_likes` Table:

|**Column Name**|**Type**|
|---|---|
|user_id|integer|
|page_id|integer|
|liked_date|datetime|

### `page_likes` Example Input:

|**user_id**|**page_id**|**liked_date**|
|---|---|---|
|111|20001|04/08/2022 00:00:00|
|121|20045|03/12/2022 00:00:00|
|156|20001|07/25/2022 00:00:00|



```sql

SELECT
  p1.page_id -- Agrupamos por page_id y no hay necesidad de DISTINCT por el left join
FROM
  pages p1
LEFT JOIN  -- Generamos un outer join en este caso para poder tener todas las ids de pages y en caso que aparezca vacio a la derecha eso quiere decir que la pagina no tiene like
  page_likes  p2
ON  
  p1.page_id = p2.page_id -- utilizamos page_id para poder cruzar ambas tablas
WHERE
  p2.liked_date IS NULL -- Filtramos por aquellos registros donde no existe un registro de un me gusta
ORDER BY 
  page_id; -- Ordenamos por page_id
  

```

""Está bien, ya sabemos cuáles tienen cero likes. Ahora quiero que me des un reporte de todas las páginas, pero con una columna extra llamada popularidad.""

Si la página tiene más de 1 like, ponle 'Popular'.

Si tiene exactamente 1 like, ponle 'Emergente'.

Si tiene 0 likes, ponle 'Inactiva'.

Ordena el resultado por las más populares primero."

```sql

-- tabla  donde contamos los likes totales
WITH likes_counter AS (
SELECT
  p1.page_id,
  page_name,
  COUNT(liked_date) AS total_likes
FROM
  pages p1
LEFT JOIN 
  page_likes  p2
ON  
  p1.page_id = p2.page_id
  
GROUP BY  
  p1.page_id,
  page_name)
  
-- utiliamos la tabla anterior pra poder generar la columna nueva
SELECT
  page_id,
  page_name,
  CASE 
    WHEN total_likes > 1 THEN 'Popular'
    WHEN total_likes = 1 THEN 'Emergente'
    ELSE 'Inactiva'
  END AS Popularidad
FROM
  likes_counter;
```