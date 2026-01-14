This is the same question as problem #28 in the SQL Chapter of [Ace the Data Science Interview](https://amzn.to/3kF79Fx)!

Assume you're given a table with measurement values obtained from a Google sensor over multiple days with measurements taken multiple times within each day.

Write a query to calculate the sum of odd-numbered and even-numbered measurements separately for a particular day and display the results in two different columns. Refer to the Example Output below for the desired format.

Definition:

- Within a day, measurements taken at 1st, 3rd, and 5th times are considered odd-numbered measurements, and measurements taken at 2nd, 4th, and 6th times are considered even-numbered measurements.

_Effective April 15th, 2023, the question and solution for this question have been revised._

### `measurements` Table:

|Column Name|Type|
|---|---|
|measurement_id|integer|
|measurement_value|decimal|
|measurement_time|datetime|

### `measurements` Example Input:

|measurement_id|measurement_value|measurement_time|
|---|---|---|
|131233|1109.51|07/10/2022 09:00:00|
|135211|1662.74|07/10/2022 11:00:00|
|523542|1246.24|07/10/2022 13:15:00|
|143562|1124.50|07/11/2022 15:00:00|
|346462|1234.14|07/11/2022 16:45:00|

# Respuesta

```sql

WITH rank_measurement AS (
    -- Genera ranking usando la los dias mes y año ordenandolos por la hora
  SELECT
    *,
    ROW_NUMBER() OVER(PARTITION BY 
                  TO_CHAR(measurement_time, 'DD/MM/YYYY')::DATE
                ORDER BY
                  measurement_time ASC)
  FROM measurements
)

-- Consulta final que suma según si son pares o impares
SELECT
  TO_CHAR(measurement_time, 'DD/MM/YYYY')::DATE AS measurement_day,
  SUM(CASE
          WHEN row_number % 2 = 0 THEN 0 ELSE measurement_value END) AS odd_sum,
  SUM(CASE
          WHEN row_number % 2 = 0 THEN measurement_value ELSE 0 END) AS even_sum

FROM
  rank_measurement
GROUP BY
  measurement_day
  
ORDER BY
  measurement_day ASC

```