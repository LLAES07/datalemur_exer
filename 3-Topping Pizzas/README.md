You’re a consultant for a major pizza chain that will be running a promotion where all 3-topping pizzas will be sold for a fixed price, and are trying to understand the costs involved.

Given a list of pizza toppings, consider all the possible 3-topping pizzas, and print out the total cost of those 3 toppings. Sort the results with the highest total cost on the top followed by pizza toppings in ascending order.

Break ties by listing the ingredients in alphabetical order, starting from the first ingredient, followed by the second and third.

_P.S. Be careful with the spacing (or lack of) between each ingredient. Refer to our Example Output._

**Notes:**

- Do not display pizzas where a topping is repeated. For example, ‘Pepperoni,Pepperoni,Onion Pizza’.
- Ingredients must be listed in alphabetical order. For example, 'Chicken,Onions,Sausage'. 'Onion,Sausage,Chicken' is not acceptable.

### `pizza_toppings` Table:

|**Column Name**|**Type**|
|---|---|
|topping_name|varchar(255)|
|ingredient_cost|decimal(10,2)|

### `pizza_toppings` Example Input:

| **topping_name** | **ingredient_cost** |
| ---------------- | ------------------- |
| Pepperoni        | 0.50                |
| Sausage          | 0.70                |
| Chicken          | 0.55                |
| Extra Cheese     | 0.40                |


# Respuesta

```sql

SELECT
  CONCAT(t1.topping_name, ',', t2.topping_name, ',', t3.topping_name) AS pizza,
  (t1.ingredient_cost + t2.ingredient_cost + t3.ingredient_cost) AS total_cost
FROM pizza_toppings t1
CROSS JOIN pizza_toppings t2
CROSS JOIN pizza_toppings t3
WHERE t1.topping_name < t2.topping_name
  AND t2.topping_name < t3.topping_name
ORDER BY total_cost DESC, pizza;


```