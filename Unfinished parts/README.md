Tesla is investigating production bottlenecks and they need your help to extract the relevant data. Write a query to determine which parts have begun the assembly process but are not yet finished.

Assumptions:

- `parts_assembly` table contains all parts currently in production, each at varying stages of the assembly process.
- An unfinished part is one that lacks a `finish_date`.

This question is straightforward, so let's approach it with simplicity in both thinking and solution.

_Effective April 11th 2023, the problem statement and assumptions were updated to enhance clarity._

### `parts_assembly` Table

|Column Name|Type|
|---|---|
|part|string|
|finish_date|datetime|
|assembly_step|integer|

### `parts_assembly` Example Input

|part|finish_date|assembly_step|
|---|---|---|
|battery|01/22/2022 00:00:00|1|
|battery|02/22/2022 00:00:00|2|
|battery|03/22/2022 00:00:00|3|
|bumper|01/22/2022 00:00:00|1|
|bumper|02/22/2022 00:00:00|2|
|bumper||3|
|bumper||4|



# RESPUESTA

```sql
SELECT 
  part,
  assembly_step
FROM 
  parts_assembly
WHERE
  finish_date IS NULL; -- Debido a que si no tiene fecha de finalizado se considera que no esta finalizado entonces buscamos los nulos y dado que si estan en la tabla quiere decir que se comenzaron pero aún no estan finalizado por la falta de fecha

```


El Reto: "Análisis de Eficiencia de Producción"

Siguiendo con el escenario de Tesla, el reclutador ahora quiere algo más analítico para detectar dónde está el cuello de botella real.

**El Problema:** Tesla quiere saber cuánto tiempo (en días) le está tomando a cada pieza completar **cada paso** de su ensamblaje.

**Tu tarea:** Escribe una consulta que devuelva:

1. El nombre de la pieza (`part`).
    
2. El paso de ensamblaje (`assembly_step`).
    
3. Una columna calculada llamada `days_taken` que sea la diferencia entre la `finish_date` de **ese paso** y la `finish_date` del **paso anterior** para la misma pieza.


# RESPUESTA

```sql
SELECT 
  part,
  assembly_step,
  finish_date - 
  LAG(finish_date) OVER(PARTITION BY part ORDER BY assembly_step ASC) AS days_taken -- Utilizo LAG para hacer una comparación de las fechas entre columnas ya que si lo particiono por la etapa del ensamble puedo comparar cada fecha de finalizacion con cada parte y reiniciando la cuenta para cada uno,
FROM 
  parts_assembly
WHERE
  finish_date IS NOT NULL;

```