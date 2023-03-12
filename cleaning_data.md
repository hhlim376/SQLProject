What issues will you address by cleaning the data?
  
- Remove irrelavant/redundant data  
- Transform price data --> divide by 1,000,000    
- Change data type in "date" columns from INTEGER to DATE  
- Change data type in "units_sold" column from VARCHAR to INTEGER  
- Remove Duplicate Rows from tables
- Clean up missing/NULL values for certain columns




Queries:
Below, provide the SQL queries you used to clean your data.
  
```sql  
--remove duplicate entries from all_sessions and analytics
CREATE TEMP TABLE all_distinct AS
SELECT DISTINCT * FROM all_sessions

CREATE TEMP TABLE analytics_distinct AS
SELECT DISTINCT * FROM analytics
```

```sql   
--Create 2 new tables, all_sessions2 and analytics2 which includes relevant data (Excluded columns with irrelavant/redundant data). Also divided product price and unit price to more accurately reflect the prices, rounded to 2 decimal places for clarity. 
CREATE TABLE all_sessions2 AS
	SELECT "fullVisitorId", "channelGrouping", country, city, Round(("totalTransactionRevenue"/1000000),2) as totaltransactionrevenue, "transactions", "timeOnSite" "pageviews", date, "visitId", "type", Round(("productPrice"/1000000),2) as productprice, Round(("productRevenue"/1000000),2) as productrevenue, "productSKU", "v2ProductName" as productname, "v2ProductCategory" as productcategory, "pageTitle" 
	FROM all_distinct

CREATE TABLE analytics2 AS
	SELECT "visitNumber", "visitId", "visitStartTime", "date", "fullvisitorId", "userid", "channelGrouping", "socialEngagementType", "units_sold", "pageviews", "timeonsite", "bounces", Round(("revenue"/1000000),2) as revenue, Round(("unit_price"/1000000),2) as unitprice 
	FROM analytics_distinct

-- Remove rows with null data from totaltransactionrevenue and revenue as well as remove duplicate rows by creating temporary tables to work with

CREATE TEMP TABLE allclean AS
SELECT * 
FROM all_sessions2
WHERE totaltransactionrevenue is not null

CREATE TEMP TABLE analyticsclean AS
SELECT * 
FROM analytics2
WHERE "units_sold" is not null

-- Realized units_sold column was the wrong datatype (Character Varying) so converted it to integer instead  

ALTER TABLE analytics2
ALTER column "units_sold" TYPE INT USING ("units_sold"::INT)


```

