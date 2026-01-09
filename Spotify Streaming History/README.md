You're given two tables containing data on Spotify users' streaming activity: `songs_history` which has historical streaming data, and `songs_weekly` which has data from the current week.

Write a query that outputs the user ID, song ID, and cumulative count of song plays up to August 4th, 2022, sorted in descending order.

Assume that there may be new users or songs in the `songs_weekly` table that are not present in the `songs_history` table.

Definitions:

- `song_weekly`table only contains data for the week of August 1st to August 7th, 2022.
- `songs_history` table contains data up to July 31st, 2022. The query should include historical data from this table.

### `songs_history` Table:

|**Column Name**|**Type**|
|---|---|
|history_id|integer|
|user_id|integer|
|song_id|integer|
|song_plays|integer|

### `songs_history` Example Input:

|**history_id**|**user_id**|**song_id**|**song_plays**|
|---|---|---|---|
|10011|777|1238|11|
|12452|695|4520|1|

`song_plays` field contains the historical data of the number of times a user has played a particular song.

### `songs_weekly` Table:

|**Column Name**|**Type**|
|---|---|
|user_id|integer|
|song_id|integer|
|listen_time|datetime|

### `songs_weekly` Example Input:

| **user_id** | **song_id** | **listen_time**     |
| ----------- | ----------- | ------------------- |
| 777         | 1238        | 08/01/2022 12:00:00 |
| 695         | 4520        | 08/04/2022 08:00:00 |
| 125         | 9630        | 08/04/2022 16:00:00 |
| 695         | 9852        | 08/07/2022 12:00:00 |