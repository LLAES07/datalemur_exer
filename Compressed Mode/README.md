You're given a table containing the item count for each order on Alibaba, along with the frequency of orders that have the same item count. Write a query to retrieve the mode of the order occurrences. Additionally, if there are multiple item counts with the same mode, the results should be sorted in ascending order.

Clarifications:

- `item_count`: Represents the number of items sold in each order.
- `order_occurrences`: Represents the frequency of orders with the corresponding number of items sold per order.
- For example, if there are 800 orders with 3 items sold in each order, the record would have an `item_count` of 3 and an `order_occurrences` of 800.

_Effective June 14th, 2023, the problem statement has been revised and additional clarification have been added for clarity._

### `items_per_order` Table:

|Column Name|Type|
|---|---|
|item_count|integer|
|order_occurrences|integer|

### `items_per_order` Example Input:

|item_count|order_occurrences|
|---|---|
|1|500|
|2|1000|
|3|800|