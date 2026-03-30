### __2026-03-29 - *DinnerDB Queries*__

## __**Context:**__
+ This file details the queries and results referenced in the *"Database Creation and Configuration"* section of `ILS_Creation.md`
+ The queries were pulled from the LIS624 textbook, and can be found [here]( https://cseanburns.github.io/systems-librarianship/5a-introduction-to-relational-databases.html)

## __**Code:**__


```
[DinnerDB]> select * from Meals;
+---------+---------------------+---------+--------------+------------+
| meal_id | meal_name           | cuisine | cooking_time | vegetarian |
+---------+---------------------+---------+--------------+------------+
|       1 | Spaghetti Bolognese | Italian |           45 |          0 |
|       2 | Vegetable Stir Fry  | Chinese |           20 |          1 |
|       3 | Chicken Curry       | Indian  |           50 |          0 |
|       4 | Mushroom Risotto    | Italian |           35 |          1 |
+---------+---------------------+---------+--------------+------------+
4 rows in set (0.00 sec)  
```

```
[DinnerDB]> select * from Meals where vegetarian = TRUE;
+---------+--------------------+---------+--------------+------------+
| meal_id | meal_name          | cuisine | cooking_time | vegetarian |
+---------+--------------------+---------+--------------+------------+
|       2 | Vegetable Stir Fry | Chinese |           20 |          1 |
|       4 | Mushroom Risotto   | Italian |           35 |          1 |
+---------+--------------------+---------+--------------+------------+
2 rows in set (0.00 sec)  
```

```
[DinnerDB]> select * from Meals order by cooking_time desc;
+---------+---------------------+---------+--------------+------------+
| meal_id | meal_name           | cuisine | cooking_time | vegetarian |
+---------+---------------------+---------+--------------+------------+
|       3 | Chicken Curry       | Indian  |           50 |          0 |
|       1 | Spaghetti Bolognese | Italian |           45 |          0 |
|       4 | Mushroom Risotto    | Italian |           35 |          1 |
|       2 | Vegetable Stir Fry  | Chinese |           20 |          1 |
+---------+---------------------+---------+--------------+------------+
4 rows in set (0.00 sec)  
```

```
[DinnerDB]> select * from Meals order by cooking_time asc;
+---------+---------------------+---------+--------------+------------+
| meal_id | meal_name           | cuisine | cooking_time | vegetarian |
+---------+---------------------+---------+--------------+------------+
|       2 | Vegetable Stir Fry  | Chinese |           20 |          1 |
|       4 | Mushroom Risotto    | Italian |           35 |          1 |
|       1 | Spaghetti Bolognese | Italian |           45 |          0 |
|       3 | Chicken Curry       | Indian  |           50 |          0 |
+---------+---------------------+---------+--------------+------------+
4 rows in set (0.00 sec)  
```

```
[DinnerDB]> select Meals.meal_name as Meals,
    ->     Ingredients.ingredient_name as Ingredients,
    ->     Ingredients.quantity as Quantity
    ->     from Meals
    ->     join Ingredients on Meals.meal_id = Ingredients.meal_id;
+---------------------+-----------------+----------+
| Meals               | Ingredients     | Quantity |
+---------------------+-----------------+----------+
| Spaghetti Bolognese | Spaghetti       | 200g     |
| Spaghetti Bolognese | Ground Beef     | 250g     |
| Spaghetti Bolognese | Tomato Sauce    | 1 cup    |
| Vegetable Stir Fry  | Broccoli        | 100g     |
| Vegetable Stir Fry  | Carrots         | 50g      |
| Vegetable Stir Fry  | Soy Sauce       | 2T       |
| Chicken Curry       | Chicken Breast  | 300g     |
| Chicken Curry       | Curry Powder    | 2T       |
| Chicken Curry       | Coconut Milk    | 1 cup    |
| Mushroom Risotto    | Arborio Rice    | 1 cup    |
| Mushroom Risotto    | Mushrooms       | 1 cup    |
| Mushroom Risotto    | Parmesan Cheese | 1/2 cup  |
+---------------------+-----------------+----------+
12 rows in set (0.00 sec)  
```

```
[DinnerDB]> select ingredient_name as Ingredients,
    ->     quantity as Quantity
    ->     from Ingredients 
    ->     where meal_id = (select meal_id from Meals where meal_name = 'Chicken Curry');
+----------------+----------+
| Ingredients    | Quantity |
+----------------+----------+
| Chicken Breast | 300g     |
| Curry Powder   | 2T       |
| Coconut Milk   | 1 cup    |
+----------------+----------+
3 rows in set (0.00 sec)  
```

```
[DinnerDB]> select cuisine, count(*) as meal_count 
    ->     from Meals
    ->     group by cuisine;
+---------+------------+
| cuisine | meal_count |
+---------+------------+
| Italian |          2 |
| Chinese |          1 |
| Indian  |          1 |
+---------+------------+
3 rows in set (0.00 sec)  
```

```
[DinnerDB]> select meal_name, cooking_time 
    ->     from Meals 
    ->     where cooking_time <= 45
    ->     order by cooking_time asc;
+---------------------+--------------+
| meal_name           | cooking_time |
+---------------------+--------------+
| Vegetable Stir Fry  |           20 |
| Mushroom Risotto    |           35 |
| Spaghetti Bolognese |           45 |
+---------------------+--------------+
3 rows in set (0.00 sec)  
```
