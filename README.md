
# SQL Query Analysis for Pizza Sales Data

## 1. Total Number of Orders
**Query:**
```sql
SELECT COUNT(order_id) AS total_orders FROM orders;
```
**Analysis:**
This query retrieves the total number of orders placed by counting the `order_id` from the `orders` table.

## 2. Total Revenue Generated
**Query:**
```sql
SELECT ROUND(SUM(order_details.quantity * pizzas.price), 2) AS total_revenue FROM order_details JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id;
```
**Analysis:**
The query calculates the total revenue by summing up the product of `quantity` and `price` of each pizza ordered.

## 3. Highest-Priced Pizza
**Query:**
```sql
SELECT MAX(pizzas.price) AS price, pizza_types.name FROM pizzas JOIN pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id GROUP BY pizza_types.name ORDER BY price DESC LIMIT 1;
```
**Analysis:**
This query finds the most expensive pizza by joining `pizzas` and `pizza_types`, sorting by price in descending order, and selecting the top record.

## 4. Most Common Pizza Size Ordered
**Query:**
```sql
SELECT pizzas.size, COUNT(order_details.quantity) AS Total_Orders FROM pizzas JOIN order_details ON pizzas.pizza_id = order_details.pizza_id GROUP BY pizzas.size ORDER BY Total_Orders DESC;
```
**Analysis:**
The query determines the most frequently ordered pizza size by counting the total quantities ordered for each size.

## 5. Top 5 Most Ordered Pizza Types
**Query:**
```sql
SELECT pizza_types.name, SUM(order_details.quantity) AS Total FROM pizza_types JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id JOIN order_details ON order_details.pizza_id = pizzas.pizza_id GROUP BY pizza_types.name ORDER BY Total DESC LIMIT 5;
```
**Analysis:**
This query identifies the five most popular pizza types based on total quantity ordered.

## 6. Category-Wise Distribution of Pizza Orders
**Query:**
```sql
SELECT pizza_types.category, SUM(order_details.quantity) AS quantity FROM pizza_types JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id JOIN order_details ON pizzas.pizza_id = order_details.pizza_id GROUP BY pizza_types.category ORDER BY quantity DESC;
```
**Analysis:**
The query categorizes pizzas based on their type (e.g., Vegetarian, Meat) and calculates the total quantity ordered for each category.

## 7. Orders Distribution by Hour
**Query:**
```sql
SELECT HOUR(order_time) AS Hours, COUNT(order_id) AS Orders FROM orders GROUP BY Hours;
```
**Analysis:**
This query helps analyze the distribution of orders across different hours of the day.

## 8. Category-Wise Pizza Distribution
**Query:**
```sql
SELECT category, COUNT(name) FROM pizza_types GROUP BY category;
```
**Analysis:**
This query shows how different pizza types are distributed across various categories.

## 9. Average Number of Pizzas Ordered per Day
**Query:**
```sql
SELECT ROUND(AVG(quantity)) FROM (SELECT orders.order_date, SUM(order_details.quantity) AS quantity FROM orders JOIN order_details ON orders.order_id = order_details.order_id GROUP BY orders.order_date) AS orders_quantity;
```
**Analysis:**
The query calculates the average number of pizzas ordered per day by aggregating daily orders first and then computing the average.

## 10. Top 3 Pizza Types by Revenue
**Query:**
```sql
SELECT pizza_types.name, SUM(order_details.quantity * pizzas.price) AS revenue FROM pizza_types JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id JOIN order_details ON order_details.pizza_id = pizzas.pizza_id GROUP BY pizza_types.name ORDER BY revenue DESC LIMIT 3;
```
**Analysis:**
This query identifies the top three pizza types based on total revenue.

## 11. Percentage Contribution of Each Pizza Type to Total Revenue
**Query:**
```sql
SELECT pizza_types.category, (SUM(order_details.quantity * pizzas.price) / (SELECT ROUND(SUM(order_details.quantity * pizzas.price), 2) AS total_sales FROM order_details JOIN pizzas ON pizzas.pizza_id = order_details.pizza_id)) * 100 AS revenue FROM pizza_types JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id JOIN order_details ON order_details.pizza_id = pizzas.pizza_id GROUP BY pizza_types.category ORDER BY revenue DESC;
```
**Analysis:**
The query calculates the percentage contribution of each pizza type to the total revenue by summing up the revenue per category and dividing it by total sales.

## 12. Cumulative Revenue Over Time
**Query:**
```sql
SELECT order_date, SUM(revenue) OVER(ORDER BY order_date) AS cum_revenue FROM (SELECT SUM(order_details.quantity * pizzas.price) AS revenue, orders.order_date FROM order_details JOIN pizzas ON pizzas.pizza_id = order_details.pizza_id JOIN orders ON orders.order_id = order_details.order_id GROUP BY orders.order_date) AS sales;
```
**Analysis:**
This query calculates cumulative revenue over time by summing daily revenues in an ordered manner.
```
