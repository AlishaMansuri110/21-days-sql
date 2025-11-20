1. List all unique pizza categories (DISTINCT).

SELECT DISTINCT category
FROM pizza_types;

2. Display pizza_type_id, name, and ingredients, replacing NULL ingredients with "Missing Data". Show first 5 rows.

SELECT 
    pizza_type_id,
    name,
    COALESCE(ingredients, 'Missing Data') AS ingredients
FROM pizza_types
LIMIT 5;

3. Check for pizzas missing a price (IS NULL).

SELECT *
FROM pizzas
WHERE price IS NULL;

4. Orders placed on '2015-01-01' (SELECT + WHERE).

SELECT *
FROM orders
WHERE date = '2015-01-01';

5. List pizzas by price descending

SELECT *
FROM pizzas
ORDER BY price DESC;

6. Pizzas with size L or XL

SELECT *
FROM pizzas
WHERE size IN ('L', 'XL');

7. Pizzas priced between 15 and 17

SELECT *
FROM pizzas
WHERE price BETWEEN 15 AND 17;

8. Pizzas with “Chicken” in the name

SELECT *
FROM pizza_types
WHERE name LIKE '%Chicken%';

9. Orders on '2015-02-15' OR placed after 8 PM

SELECT *
FROM orders
WHERE date = '2015-02-15'
   OR CAST(LEFT(time, 2) AS UNSIGNED) > 20;

10. Total quantity sold

SELECT SUM(quantity) AS total_pizzas_sold
FROM order_details;

11. Average pizza price

SELECT AVG(price) AS avg_pizza_price
FROM pizzas;

12. Total order value per order

SELECT 
    od.order_id,
    SUM(od.quantity * p.price) AS total_order_value
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id
GROUP BY od.order_id;

13. Total quantity sold per pizza category

SELECT 
    pt.category,
    SUM(od.quantity) AS total_sold
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.category;

14. Categories with more than 5000 sold

SELECT 
    pt.category,
    SUM(od.quantity) AS total_sold
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.category
HAVING SUM(od.quantity) > 5000;

15. Pizzas never ordered

SELECT p.pizza_id, p.size, p.price
FROM pizzas p
LEFT JOIN order_details od ON p.pizza_id = od.pizza_id
WHERE od.order_id IS NULL;

16. Price difference between sizes (Self JOIN)

SELECT 
    p1.pizza_type_id,
    p1.size AS size1,
    p2.size AS size2,
    (p2.price - p1.price) AS price_difference
FROM pizzas p1
JOIN pizzas p2 
      ON p1.pizza_type_id = p2.pizza_type_id
     AND p1.size < p2.size;
