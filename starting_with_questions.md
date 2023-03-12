Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
```sql  
SELECT "country", SUM (totaltransactionrevenue) AS sumrevenue
FROM allclean
GROUP BY "country"
ORDER BY sumrevenue DESC  
  
SELECT "city", SUM (totaltransactionrevenue) AS sumrevenue
FROM allclean
GROUP BY "city"
ORDER BY sumrevenue DESC  
```

Answer:

Country: United States  

City: San Fransisco  
  


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
```sql
-- Assuming that products ordered can be derived from the units_sold column under analytics, we compare country from all_sessions table with units_sold from analytics table, joining on visitor Id
WITH t1 AS(
	SELECT "fullVisitorId", "country", "city", "units_sold"
	FROM all_sessions2 as2
	INNER JOIN analyticsclean ac
	ON as2."fullVisitorId" = ac."fullvisitorId"
)

SELECT "country", ROUND(AVG(units_sold),2) as avg_sold
FROM t1
GROUP BY "country"
ORDER BY avg_sold DESC

--Same but with cities
WITH t1 AS(
	SELECT "fullVisitorId", "country", "city", "units_sold"
	FROM all_sessions2 as2
	INNER JOIN analyticsclean ac
	ON as2."fullVisitorId" = ac."fullvisitorId"
)

SELECT "city", ROUND(AVG(units_sold),2) as avg_sold
FROM t1
GROUP BY "city"
ORDER BY avg_sold DESC
```


Answer:  
NOTE: This is working under the assumption that units_sold refers to products ordered  
  
Country: United States (26), Canada (2), Germany/Japan/Switzerland (1)  

City: San Bruno (61), Mountain View (24), New York (10) ..... Yokohama (1)





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

```sql
--t1 gives us information on people who placed an order on a product (units_sold not null). We then transform the data to see how many products from a specific category was ordered based on country.
WITH t1 AS 
	(SELECT "fullVisitorId", "country", "city", "v2ProductName" AS product_name, "v2ProductCategory" AS product_category, "units_sold"
		FROM all_sessions a1
		INNER JOIN analyticsclean a2
		ON a1."fullVisitorId" = a2."fullvisitorId"
		WHERE units_sold IS NOT NULL
		ORDER BY "fullVisitorId"
		)
		
SELECT "country", product_category, COUNT("country") AS products_ordered
FROM t1
GROUP BY product_category, "country"
ORDER BY "country"

--Same as above but with city

WITH t1 AS 
	(SELECT "fullVisitorId", "country", "city", "v2ProductName" AS product_name, "v2ProductCategory" AS product_category, "units_sold"
		FROM all_sessions a1
		INNER JOIN analyticsclean a2
		ON a1."fullVisitorId" = a2."fullvisitorId"
		WHERE units_sold IS NOT NULL
		ORDER BY "fullVisitorId"
		)
		
SELECT "city", product_category, COUNT("country") AS products_ordered
FROM t1
GROUP BY product_category, "city"
ORDER BY "city"

```


Answer:  

Some Trends from the results  
- Nest-USA was the most popular category for American buyers
- Alot of people from Charlotte bought bags 
- Alot of houseware purchases from Sunnyvale

 




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```sql 
-- Adding up all the units_sold, grouping by country and product name 
WITH t1 AS 
	(SELECT "fullVisitorId", "country", "city", "v2ProductName" AS product_name, "v2ProductCategory" AS product_category, "units_sold"
		FROM all_sessions a1
		INNER JOIN analyticsclean a2
		ON a1."fullVisitorId" = a2."fullvisitorId"
		WHERE units_sold IS NOT NULL
		ORDER BY "fullVisitorId"
		)

SELECT "country", product_name, SUM(Units_sold) AS sum_sold
FROM t1
GROUP BY "country", product_name
ORDER BY "country", sum_sold DESC

-- Same but with cities
WITH t1 AS 
	(SELECT "fullVisitorId", "country", "city", "v2ProductName" AS product_name, "v2ProductCategory" AS product_category, "units_sold"
		FROM all_sessions a1
		INNER JOIN analyticsclean a2
		ON a1."fullVisitorId" = a2."fullvisitorId"
		WHERE units_sold IS NOT NULL
		ORDER BY "fullVisitorId"
		)

SELECT "city", product_name, SUM(Units_sold) AS sum_sold
FROM t1
GROUP BY "city", product_name
ORDER BY "city", sum_sold DESC

```




Answer:  
Country: Canada (Google Men's 3/4 Sleeve Raglan Henley Grey), United States (Straw Beach Mat) .... Switzerland (YouTube Men's 3/4 Sleeve Henley)  

City: Ann Arbor (Recycled Mouse Pad), Atlanta (Google Onesie Green), Austin (YouTube RFID Journal) ... Zurich (YouTube Men's 3/4 Sleeve Henley)





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
```sql
-- Calculate total revenue generated from purchases made in different countries
WITH t1 AS
	(SELECT "fullVisitorId", "country", "city", "v2ProductName" AS product_name, "v2ProductCategory" AS product_category, "units_sold", revenue
		FROM all_sessions a1
		INNER JOIN analyticsclean a2
		ON a1."fullVisitorId" = a2."fullvisitorId"
		WHERE units_sold IS NOT NULL
		ORDER BY "fullVisitorId"
	  )
	  
SELECT "country", SUM(revenue) AS revenue_generated
FROM t1
GROUP BY "country"
ORDER BY revenue_generated DESC  

--Same but for cities
WITH t1 AS
	(SELECT "fullVisitorId", "country", "city", "v2ProductName" AS product_name, "v2ProductCategory" AS product_category, "units_sold", revenue
		FROM all_sessions a1
		INNER JOIN analyticsclean a2
		ON a1."fullVisitorId" = a2."fullvisitorId"
		WHERE units_sold IS NOT NULL
		ORDER BY "fullVisitorId"
	  )
	  
SELECT "city", SUM(revenue) AS revenue_generated
FROM t1
GROUP BY "city"
ORDER BY revenue_generated DESC
```


Answer:  
The highest amount of revenue was generated by the U.S. for countries and Mountain View for cities. We can look at the query output for details on the other countries and cities as well.







