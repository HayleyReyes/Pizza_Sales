# Pizza_Sales

### Project Overview

In this exciting project, I'm delving into the world of a Pizza Company, where I am – designing a relational database, crafting advanced custom SQL queries, and bringing data to life with an interactive PowerBI dashboard.

### Objective 
This project aims to identify sales trends, make data-driven recommendations, adn gain deeper understanding of the company's performance. 

### Data Source 
The primary dataset source used for this analysis is "pizza_proj" folder that will be uploaded. 

### Tools
  - QuickDBD - Designing relational database
  - MySQL - Data analysis
  - Looker Studio - Creating a visualization
### Data Analysis
Order Activity
-	Total Orders
-	Total Sales
-	Total Items
-	Average order Value
-	Sales by category
-	Top selling items
-	Orders by hour
-	Orders by address
  
Inventory Management
-	Total quantity by ingredients
-	Total coast of ingredients
-	Calculated cost of ingredients
-	Percentage stock remaining by ingredients
  
Staff 
-	Hours in shift
-	Hourly rate 
-	Staff cost 
-	Start and end times
### Queries
```sql
Pizza Project 1 Query

SELECT 
	o.order_id,
	i.item_price,
	o.quantity,
	i.item_cat,
	i.item_name,
	o.created_at,
	a.delivery_address1,
	a.delivery_address2, 
	a.delivery_city,
	a.delivery_zipcode,
	o.delivery
FROM 
	orders o 
	LEFT JOIN item i ON o.item_id = i.item_id
	LEFT JOIN address a ON o.add_id = a.add_id

Pizza Project 2 Query

SELECT 
s1.item_name, 
s1.ing_name,
s1.ing_id,
s1.ing_weight,
s1.ing_price,
s1.order_quantity,
s1.recipe_quantity,
s1.order_quantity*s1.recipe_quantity as ordered_weight,
s1.ing_price/s1.ing_weight as unit_cost,
(s1.order_quantity*s1.recipe_quantity)*(s1.ing_price/s1.ing_weight) as ingredient_cost
from (SELECT 
	o.item_id, 
	i.sku, 
	item_name, 
	ing.ing_name,
	r.ing_id,
	r.quantity as recipe_quantity,
	sum(o.quantity) as order_quantity,
	ing.ing_weight,
	ing.ing_price
FROM 
	orders o
	LEFT JOIN item i ON o.item_id = i.item_id
	LEFT JOIN recipe r ON i.sku = r.recipe_id
	LEFT JOIN ingredient ing ON r.ing_id = ing.ing_id
GROUP BY o.item_id, 
	i.sku, 
	i.item_name, 
	r.ing_id,
	r.quantity,
	ing.ing_name,
	ing.ing_weight,
	ing.ing_price) as s1

Pizza Project 3 Query

SELECT
	s2.ing_name, 
	s2.ordered_weight,
	ing.ing_weight*inv.quantity as total_inv_weight, 
	(ing.ing_weight * inv.quantity) -s2.ordered_weight as remaining_weight
FROM(SELECT 
	ing_id,
	ing_name,
	sum(ordered_weight) as ordered_weight
	FROM 
	stock11
GROUP BY
	ing_name,
	ing_id) s2
LEFT JOIN inventory inv ON inv.item_id = s2.ing_id
LEFT JOIN ingredient ing ON ing.ing_id = s2.ing_id

Pizza Project 4 Query
SELECT 
r.date,
s.first_name,
s.last_name,
s.hourly_rate,
sh.start_time,
sh.end_time,
((hour(timediff(sh.end_time,sh.start_time))*60)+(minute(TIMEDIFF(sh.end_time,sh.start_time))))/60 as hours_in_shift,
((hour(timediff(sh.end_time,sh.start_time))*60)+(minute(TIMEDIFF(sh.end_time,sh.start_time))))/60 *s.hourly_rate as staff_cost
FROM 
rota r 
LEFT JOIN staff s ON s.staff_id = r.staff_id
LEFT JOIN shift sh ON r.shift_id = sh.shift_id
```
### Findings/Results
 - We found that peak sales times were 1 p.m and 7 p.m of the locations
 - The average order value is $102.88
 - Item Pizza Quattro Formation has the highest sales and is the most popular by demand
 - Highest sales location is located in Central Manchester
 - We were able to determine remaining stock of ingredients along with cost
 - Staffing suggestions and total staffing costs

### Recommmendations
Based on our findings, it is recommended to:
 - Invest in marketing and promotions during the peak sales hours. Also to add promotions during outside of peak time to increase revenue
 - Focus on expanding and promoting top sale items (Pizza Quattro Formation and Pizza Daviola)
### Limitations 
 - N/A









