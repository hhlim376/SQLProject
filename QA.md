What are your risk areas? Identify and describe them.  
- Dataset has many missing entries/NULL values
- Some data seems to be duplicated and needs to be cleaned
- Some values would need to be transformed to make more sense (e.g. most of the prices should be divided by 1,000,000)


QA Process:
Describe your QA process and include the SQL queries used to execute it.

- Tried to make queries easy to read and easy to understand, added notes to more complicated queries to explain what I'm trying to do with the code   
- When removing rows with NULL information, query the output to make sure rows are actually removed

```sql
-- Example, after generating this table
SELECT "fullVisitorId", "country", "city", "v2ProductName" AS product_name, "v2ProductCategory" AS product_category, "units_sold"
		FROM all_sessions a1
		INNER JOIN analyticsclean a2
		ON a1."fullVisitorId" = a2."fullvisitorId"
		WHERE units_sold IS NOT NULL
		ORDER BY "fullVisitorId"

-- Query to make sure there are no NULL returns for units_sold
WITH t1 AS(
SELECT "fullVisitorId", "country", "city", "v2ProductName" AS product_name, "v2ProductCategory" AS product_category, "units_sold"
		FROM all_sessions a1
		INNER JOIN analyticsclean a2
		ON a1."fullVisitorId" = a2."fullvisitorId"
		WHERE units_sold IS NOT NULL
		ORDER BY "fullVisitorId"
	)
SELECT * FROM t1 WHERE units_sold IS NULL

```
- When isolating uniques, Query to check if only uniques are returned
```sql
-- Example, Checking to see if visitorid is unique (only unique visitors)
SELECT "visitorid", COUNT (*) 
FROM
	(SELECT DISTINCT "fullVisitorId" AS visitorid, "country" 
		FROM all_sessions) t1
GROUP BY "visitorid"
HAVING COUNT (*)  > 1
```
- When calculating something, run a query to ensure numbers make sense
```sql
--Example, when calculating averages...
SELECT "country", ROUND(AVG(units_sold),2) as avg_sold
FROM t1
GROUP BY "country"
ORDER BY avg_sold DESC

-- ...Can also calculate Min and Max to make sure the average falls between the Min and Max Values
SELECT "country", MAX (units_sold), MIN (units_sold)
FROM t1
GROUP BY "country"
ORDER BY "country"
```
- Make sure when joining tables that tables consist of the same data type
- When Joining tables, Check to see if Counts are making sense
```sql
-- In this case the count from the combined table should be less than the count from the original tables, which it is

SELECT COUNT ("fullVisitorId")
FROM 
	(SELECT "fullVisitorId", "country", "city", "units_sold"
		FROM all_sessions2 as2
		INNER JOIN analyticsclean ac
		ON as2."fullVisitorId" = ac."fullvisitorId") t1
--
SELECT COUNT ("fullVisitorId")
FROM all_sessions2
```