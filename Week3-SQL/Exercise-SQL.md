
# SQL:  Structured Query Language  Exercise

### Getting Started
1. Go to BigQuery UI https://console.cloud.google.com/bigquery
2. Add in the public data sets. 
	3. Click the Add Data icon
	4. Add any dataset
	5. `bigquery-public-data` should become visible and populate in the BigQuery UI. 
3. Add your queries where it says [YOUR QUERY HERE].
4. Make sure you add your query in between the triple tick marks. 
---

For this section of the exercise we will be using the `bigquery-public-data.austin_311.311_service_requests`  table. 

5. Write a query that tells us how many rows are in the table. 
	```
	Select Count(*)
    From `bigquery-public-data.austin_311.311_service_requests`
	```

7. Write a query that tells us how many _distinct_ values there are in the complaint_description column.
	``` 
	SELECT
      COUNT(DISTINCT(complaint_description))
    FROM
  `bigquery-public-data.austin_311.311_service_requests`
	```
  
8. Write a query that counts how many times each owning_department appears in the table and orders them from highest to lowest. 
	``` 
    SELECT
      owning_department AS Department,
      COUNT(owning_department) AS deptcount
    FROM
      `bigquery-public-data.austin_311.311_service_requests`
    GROUP BY
      owning_department
    ORDER BY
      deptcount DESC
	```

9. Write a query that lists the top 5 complaint_description that appear most and the amount of times they appear in this table. (hint... limit)
	```
        SELECT
      complaint_description AS Complaint,
      COUNT (complaint_description) as comp
    FROM
      `bigquery-public-data.austin_311.311_service_requests`
    GROUP BY
      complaint_description
    ORDER BY
      comp DESC
      LIMIT 5

	  ```
10. Write a query that lists and counts all the complaint_description, just for the where the owning_department is 'Animal Services Office'.
	```
    SELECT
      complaint_description AS Complaint,
      COUNT (complaint_description) AS comp
    FROM
      `bigquery-public-data.austin_311.311_service_requests`
    WHERE
      owning_department = 'Animal Services Office'
    GROUP BY
      complaint_description
    ORDER BY
      comp DESC

	```

11. Write a query to check if there are any duplicate values in the unique_key column (hint.. There are two was to do this, one is to use a temporary table for the groupby, then filter for values that have more than one count, or, using just one table but including the  `having` function). 
	```
	SELECT
      unique_key,
      COUNT (*)
    FROM
      `bigquery-public-data.austin_311.311_service_requests`
    GROUP BY
      unique_key
    HAVING
      COUNT(*) > 1
	```


### For the next question, use the `census_bureau_usa` tables.

1. Write a query that returns each zipcode and their population for 2000 and 2010. 
	```
	WITH
      ZIP_2000 AS(
      SELECT
        zipcode AS zip2000,
        population AS zpop_2000
      FROM
        `bigquery-public-data.census_bureau_usa.population_by_zip_2000`),
      ZIP_2010 AS(
      SELECT
        zipcode,
        population AS zpop_2010
      FROM
        `bigquery-public-data.census_bureau_usa.population_by_zip_2010`)

    SELECT
      zpop_2000,
      zpop_2010,zipcode
    FROM
      ZIP_2000 AS A
    JOIN
      ZIP_2010 AS B
      ON
      A.zip2000 = B.zipcode
	```

### For the next section, use the  `bigquery-public-data.google_political_ads.advertiser_weekly_spend` table.
1. Using the `advertiser_weekly_spend` table, write a query that finds the advertiser_name that spent the most in usd. 
	```
	SELECT
      advertiser_name AS rich,
      COUNT (spend_usd) as MoneyUSD

    FROM
      `bigquery-public-data.google_political_ads.advertiser_weekly_spend`

    GROUP BY
    advertiser_name

    ORDER BY
    MoneyUSD DESC
	```
2. Who was the 6th highest spender? (No need to insert query here, just type in the answer.)
	```
	SALEM WEB NETWORK, LLC
	```

3. What week_start_date had the highest spend? (No need to insert query here, just type in the answer.)
	```
	2020-09-13
	```

4. Using the `advertiser_weekly_spend` table, write a query that returns the sum of spend by week (using week_start_date) in usd for the month of August only. 
	```
	SELECT
      week_start_date AS Week,
      SUM (spend_usd) AS SumWeek

    FROM
      `bigquery-public-data.google_political_ads.advertiser_weekly_spend`

    GROUP BY
    week_start_date

    ORDER BY
    SumWeek DESC
	```
6.  How many ads did the 'TOM STEYER 2020' campaign run? (No need to insert query here, just type in the answer.)
	```
	45137
	```
7. Write a query that has, in the US region only, the total spend in usd for each advertiser_name and how many ads they ran. (Hint, you're going to have to join tables for this one). 
	```
		WITH
      USR AS(
      SELECT
        advertiser_name AS ADSName,
        total_creatives AS Total_Ads
      FROM
        `bigquery-public-data.google_political_ads.advertiser_stats`
      WHERE
        regions = 'US'),
      Only_usd AS(
      SELECT
        advertiser_name,
        spend_usd AS USD,
      FROM
        `bigquery-public-data.google_political_ads.advertiser_weekly_spend`)
    SELECT
      ADSName,
      Total_Ads,
      USD
    FROM
      USR AS A
    JOIN
      Only_usd AS B
    ON
      A.ADSName = B.advertiser_name
	```
8. For each advertiser_name, find the average spend per ad. 
	```
	SELECT
      advertiser_name AS ADName,
      AVG(spend_usd) AS Average,
      MIN(total_creatives) AS ADS
    FROM
      `bigquery-public-data.google_political_ads.advertiser_stats`
      GROUP BY
      advertiser_name

	```
10. Which advertiser_name had the lowest average spend per ad that was at least above 0. 
	``` 
	[YOUR QUERY HERE]
	```
## For this next section, use the `new_york_citibike` datasets.

1. Who went on more bike trips, Males or Females?
	```
    SELECT
      COUNT(tripduration) AS Trip
    FROM
      `bigquery-public-data.new_york_citibike.citibike_trips`
    WHERE
      gender = 'male'
	 ***Males went on more bike trips***
	```
2. What was the average, shortest, and longest bike trip taken in minutes?
	```
	SELECT
      AVG(tripduration) AS TripAverage,
      MIN(tripduration) AS MinTrip,
      MAX(tripduration) AS MaxTrip
    FROM
      `bigquery-public-data.new_york_citibike.citibike_trips`

	```

3. Write a query that, for every station_name, has the amount of trips that started there and the amount of trips that ended there. (Hint, use two temporary tables, one that counts the amount of starts, the other that counts the number of ends, and then join the two.) 
	```
	[YOUR QUERY HERE]
	```
# The next section is the Google Colab section.  
1. Open up this [this Colab notebook](https://colab.research.google.com/drive/1kHdTtuHTPEaMH32GotVum41YVdeyzQ74?usp=sharing).
2. Save a copy of it in your drive. 
3. Rename your saved version with your initials. 
4. Click the 'Share' button on the top right.  
5. Change the permissions so anyone with link can view. 
6. Copy the link and paste it right below this line. 
	* YOUR LINK:  ________________________________
9. Complete the two questions in the colab notebook file. 
