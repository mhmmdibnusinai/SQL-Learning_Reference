# IV.1. Subqueries         
**SQL: Subqueries Introduction – Compound Questions**          
**Iowa Liquor Product Sales**            

**Introduction**           
As a normal part of communications, we ask questions with many sub-parts. SQL enables you to ask compound or complex questions of stored data by using subqueries. While there are many types of subqueries, their main goal is to enable asking more questions at one time. 

Whenever you have a complex question to ask, its best to begin by breaking the overarching question into smaller components and laying them out in the order which they need to be accomplished. SQL completes the inner subqueries before it executes the outer loop. Knowing this will help you determine the appropriate order for building the components of the question(s) to achieve the desired output. In this lesson, we will discuss and demonstrate three common places to insert a subquery. They are within the following clauses:

•	`FROM` - to replace a static table with a temporary table
•	`WHERE EXISTS` – to create a dynamic filtering list
•	`WITH` table `AS` – for selecting a dynamic set of data before you begin

**Scenario**             
You are working with very large, real world data tables from Iowa.gov, containing data from anyone who sells liquor in Iowa using a class E license. A product management team from a franchise that owns several liquor stores in the state has asked you to assist them with the following analysis questions. 

1.	**Canadian Whiskies** are the second most popular purchase choice, based on the state’s data. The management team is evaluating the vendors and their suggested retail prices. Create a query that calculates the average bottle price and the number of individual products for **Canadian Whiskies** from each vendor. Use a subquery to create a temporary table from the much larger Products table, that only contains **Canadian Whiskies**.

```
SELECT vendor_name AS Canadian_Whiskies_Vendor, 
              round(AVG(bottle_price),2) AS Avg_Price,
	count(bottle_price) AS Variety_Count
FROM (
    	SELECT item_no, products.vendor_name, products.bottle_price
    	FROM products
    	WHERE category_name = 'CANADIAN WHISKIES') AS temp_table
GROUP BY vendor_name
ORDER BY Variety_Count DESC;
```
			
This analysis would be a useful part for a marketing team considering which vendors provide the most inventory for this preferred product and the typical price point for their products. The query contained within the `FROM` clause created a small temporary table that only contained data from the category of interest, and the outer loop generated aggregated values for the average price and number of different types sold by end vendor. This is a simple way to reduce the table size for rapid execution and then generate focused aggregated values.
 
2.	The product management team is specifically interested in sales for **White Rum**, particularly when it is sold in the largest counties. Build a query that lists the transactions, by county, selling **White Rum** within a county of at least 175k residents.  Take this opportunity to apply your newest SQL query tool, `WHERE EXISTS`.  Compound the `WHERE clause` with the logical operator `AND` to add the filter for **White Rum** transactions only. Your query will contain the following:         

**Subquery transactions of White Rum in counties >175000 pop**         
```
SELECT sales_2016.invoice_item_number,
       sales_2016.store_number,
       sales_2016.county_number,
       sales_2016.county,
       sales_2016.sale_dollars
 FROM sales_2016
WHERE EXISTS (
             SELECT 1
             FROM counties
             WHERE counties.population > 175000
             AND sales_2016.county_number = counties.county_number)
AND sales_2016.category_name = "White Rum"
ORDER BY sales_2016.county;
```

This query shows 5,839 transactions for the largest counties selling **White Rum**. You also observe that there are only two counties that fit into this criteria, **Polk** and **Linn**. Interestingly, you see that **Linn** county is in the data table with two different stylings (LINN and Linn). In a later query, you’ll need to adjust for this inconsistent data cleaning issue if you create aggregated values by county.
 
3.	Using the `WITH` subquery command, you can build new table for your query to draw from before your query begins. The new table’s name is called in the `FROM` clause, just as other tables are sourced. Using this technique, you can reduce the size of a very large table and improve execution time or simply pre-assemble data from various source tables for ease of use. 

For this next query, use the `WITH` subquery structure to create a query to describe how many retail outlets exist in counties with population greater than 75,000.  Also calculate the ratio of the county’s population to the number of stores.  You will need a `JOIN` statement in both your `WITH` query and the main body of the query to provide all the information required. Your query may look like this:

**How many retail outlets in counties with > 75,000 population**           
```
WITH store_list AS (
   SELECT stores.store,
     stores.name,
     stores.store_address,
     stores_convenience.county
    FROM stores JOIN stores_convenience USING (store)
)
SELECT store_list.county, counties.population, count(*) as count_retail_locations,
   	 counties.population/count(*) as ratio_population_per_store
FROM store_list JOIN counties ON store_list.county = counties.county
WHERE counties.population>75000
GROUP BY store_list.county, counties.population
ORDER BY ratio_population_per_store;
```

Upon review of this analysis, the marketing team may request additional analysis to explore counties with higher numbers for both residents and ratio of population to stores, as that may represent sufficient demand for additional retail location investment. 
