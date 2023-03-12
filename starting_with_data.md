Question 1: Which Year had the most amount of site visits?

SQL Queries:
```sql  
SELECT EXTRACT(YEAR FROM date) AS year, COUNT(EXTRACT(YEAR FROM date))
FROM all_sessions2
GROUP BY year
```


Answer: 2016 had 7004 visitors while 2017 had 8130 visitors. Increase in site traffic!  




Question 2: 
Calculate the number of Unique visitors to the site from different countries.  

SQL Queries:  
```sql  
SELECT "country", COUNT (country) AS unique_visitors
FROM (
		SELECT DISTINCT "fullVisitorId" AS visitorid, "country" 
		FROM all_sessions
		) t1
GROUP BY "country"
ORDER BY unique_visitors DESC  
```

Answer:
In total there were unique visitors from 136 different countries to the site with the most coming from the United States (8118), followed by India (693) and the UK(641)


Question 3: On average, which countries visitors spent the longest time on the site?

SQL Queries:
```sql
SELECT "country", AVG("timeOnSite") AS avg_time
FROM all_sessions
GROUP BY "country"
ORDER BY avg_time DESC
```

Answer: Peru, followed by Nigeria then Tunisia. The United States is down in 53rd, which is a little bit of a surprise



Question 4: Do a simple analysis of page views for visitors to the site

SQL Queries:
```sql
-- First pull out visitorid and pageview data where pageview is not null, then calculate the min, max and average pageviews
WITH t1 AS
	(SELECT "fullVisitorId", "pageviews"
	FROM all_sessions
	WHERE pageviews IS NOT NULL
	 )

SELECT MIN(pageviews), MAX(pageviews), AVG(pageviews)
FROM t1  

-- Create a new column with approximate pageview for each visitor (low, medium, high, very high), then do a count of this new column

WITH t1 AS
	(SELECT "fullVisitorId", "pageviews"
	FROM all_sessions
	WHERE pageviews IS NOT NULL
	 ),

t2 AS( 
SELECT "fullVisitorId", 
		pageviews, 
		CASE
			WHEN pageviews > 20 THEN 'Very High'
			WHEN pageviews > 10 AND pageviews <= 20 THEN 'High'
			WHEN pageviews > 3 AND pageviews <=10 THEN 'Medium'
			ELSE 'Low'
			END AS approx_pageviews
	FROM t1
	)

SELECT approx_pageviews, COUNT(approx_pageviews)
FROM t2
GROUP BY approx_pageviews
```

Answer: 
- The vast majority of visitors (~93%) viewed between 1-10 pages on the site (Low,medium). 
- ~6% of visitors viewed 10-20 pages on the site (High)
- Only 0.2 % of visitors viewed more than 20 pages on the site, with the highest page view count being 30 


