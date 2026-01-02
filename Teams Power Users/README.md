Write a query to identify the top 2 Power Users who sent the highest number of messages on Microsoft Teams in August 2022. Display the IDs of these 2 users along with the total number of messages they sent. Output the results in descending order based on the count of the messages.

Assumption:

- No two users have sent the same number of messages in August 2022.

### `messages` Table:

|**Column Name**|**Type**|
|---|---|
|message_id|integer|
|sender_id|integer|
|receiver_id|integer|
|content|varchar|
|sent_date|datetime|

### `messages` Example Input:

| **message_id** | **sender_id** | **receiver_id** | **content**             | **sent_date**       |
| -------------- | ------------- | --------------- | ----------------------- | ------------------- |
| 901            | 3601          | 4500            | You up?                 | 08/03/2022 00:00:00 |
| 902            | 4500          | 3601            | Only if you're buying   | 08/03/2022 00:00:00 |
| 743            | 3601          | 8752            | Let's take this offline | 06/14/2022 00:00:00 |
| 922            | 3601          | 4500            | Get on the call         | 08/10/2022 00:00:00 |


# RESPUESTA


```sql

SELECT
    -- Selecciona y cuenta la cantidad de mensajes por ususarios
  sender_id,
  COUNT(DISTINCT message_id)
FROM messages
WHERE 
    -- Filtra solamente los datos de agosto del 2022
  sent_date BETWEEN '2022-08-01' AND '2022-08-31'
GROUP BY
  sender_id
ORDER BY 
    -- Ordena de mayor a menor segun la cuenta de los mensajes
  2 DESC
    -- Filtra el top 2
LIMIT 2;

```