Given a table of candidates and their skills, you're tasked with finding the candidates best suited for an open Data Science job. You want to find candidates who are proficient in Python, Tableau, and PostgreSQL.

Write a query to list the candidates who possess all of the required skills for the job. Sort the output by candidate ID in ascending order.

Assumption:

There are no duplicates in the candidates table.
candidates Table:
Column Name	Type
candidate_id	integer
skill	varchar
candidates Example Input:
candidate_id	skill
123	Python
123	Tableau
123	PostgreSQL
234	R
234	PowerBI
234	SQL Server
345	Python
345	Tableau



# Respuesta
```sql

-- Selecciona solo las ids
SELECT
  candidate_id
FROM candidates
GROUP BY
  candidate_id -- Agrupa por las ids de los candidatos
HAVING  -- Se utiliza CASE para poder conocer las ids de los candidatos que tienen las tres habilidades requeridas en caso de tenerlas entonces se suman y con el HAVING se filtran para conocer aquellos que tengan las tres
   SUM (CASE 
    WHEN LOWER(skill) IN ('python', 'tableau', 'postgresql') THEN 1 ELSE 0 END ) = 3

```

## Alternativa

```sql

SELECT
    candidate_id

FROM
    candidates
WHERE -- Filtramos por las hábilidades solicitadas
    LOWER(skill) IN ('python', 'tableau', 'postgresql')
GROUP BY  -- Agrupamos por las ids de cada candidato de manera que cada fila agrupada se cuenta para saber cuantas de las 3 habilidades manejan
    candidate_id
HAVING -- Filtra para asegurar que solo estan las ids de los candidatos con las tres habilidades pedidas 
    COUNT(DISTINCT skill) = 3
ORDER BY candidate_id ASC;

```


## Pregunta bonus 

"No solo quiero que tengan las 3 habilidades, sino que Python es obligatorio, y de las otras dos (Tableau y PostgreSQL) solo necesitan tener al menos una".

SELECT
    candidate_id
FROM
    candidates
WHERE 
    LOWER(skill) IN ('python', 'tableau', 'postgresql')
GROUP BY
    candidate_id
HAVING 
    --Al menos 2 de las 3 habilidades en total
    COUNT(DISTINCT LOWER(skill)) >= 2 
    -- UNA de esas sea Python
    AND SUM(CASE WHEN LOWER(skill) = 'python' THEN 1 ELSE 0 END) > 0;
