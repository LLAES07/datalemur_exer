This is the same question as problem #10 in the SQL Chapter of [Ace the Data Science Interview](https://amzn.to/3kF79Fx)!

Given a table of tweet data over a specified time period, calculate the 3-day rolling average of tweets for each user. Output the user ID, tweet date, and rolling averages rounded to 2 decimal places.

Notes:

- A rolling average, also known as a moving average or running mean is a time-series technique that examines trends in data over a specified period of time.
- In this case, we want to determine how the tweet count for each user changes over a 3-day period.

_Effective April 7th, 2023, the problem statement, solution and hints for this question have been revised._

### `tweets` Table:

|Column Name|Type|
|---|---|
|user_id|integer|
|tweet_date|timestamp|
|tweet_count|integer|

### `tweets` Example Input:

| user_id | tweet_date          | tweet_count |
| ------- | ------------------- | ----------- |
| 111     | 06/01/2022 00:00:00 | 2           |
| 111     | 06/02/2022 00:00:00 | 1           |
| 111     | 06/03/2022 00:00:00 | 3           |
| 111     | 06/04/2022 00:00:00 | 4           |
| 111     | 06/05/2022 00:00:00 | 5           |


# Respuesta

````sql

SELECT
    user_id,
    tweet_date,
    ROUND(AVG(tweet_count) OVER (PARTITION BY user_id
        ORDER BY tweet_date
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ), 2) AS moving_avg
FROM tweets;


```