# IV.2. SQL Subqueries: Diving Deeper

Take your queries to the next level by combining the functions and tools!

•	Let’s build on the query about **White Rum** purchases. The executives have asked to see which stores that have sold the **White Rum**, in those largest population centers. Show the quantity of transactions and the total sales, sorted to show the largest values at the beginning of the list. Remember to adjust county name column, to prepare for any inconsistencies in how the county name is recorded in the data table. Your query will look like the following:

**White Rum sales, in +175k population counties, with qty transactions and total sales**        
```
SELECT 
       sales_2016.store_number AS store_id,
       UPPER(sales_2016.county) AS county_name,
       COUNT(1) AS number_transactions,
       SUM(sales_2016.sale_dollars) AS total_dollars
FROM sales_2016
WHERE EXISTS (
            SELECT 1
             FROM counties
             WHERE counties.population > 175000
             AND sales_2016.county_number = counties.county_number)
AND sales_2016.category_name = "White Rum"
GROUP BY UPPER(sales_2016.county), store_id
ORDER BY county_name, total_dollars DESC;
````

Your query returns 283 rows showing the stores, with their specific volume and sales details, who have successfully sold **White Rum** over the course of this year. With each county grouping, the details are sort in descending order based on the total dollars sold.

•	The team is concerned about the carrying cost of high priced items. In particular, they are concerned about any item that sells for more than $150. They want to balance the inventory cost with the demand in the market. They decide to analyze the activity within **Linn** county as a model. Combine your SQL tools to answer the question of which **Linn** county liquor stores sell products with an **MSRP** higher than $150. In addition to the `WHERE EXISTS` subquery filtering tool, use an `AND` logical operator restricting the search to only **Linn** county in the `WHERE` clause.  By doing this, the source table size is reduced from over 2 million rows to 190k rows, greatly improving the speed of execution. Your successful query will look like this: 

**Stores, in LINN county, selling the most expensive products**          
```
SELECT sales_2016.store_number,
        UPPER(sales_2016.county) AS county_name, 
        COUNT(sales_2016.sale_dollars) AS expensive_qty_sold,
        SUM(sales_2016.sale_dollars) AS expensive_total_dollars
FROM sales_2016
WHERE EXISTS (
            SELECT 1
            FROM products 
            WHERE products.bottle_price>150
            AND sales_2016.item_number=products.item_no) 
AND sales_2016.county_number = 57
GROUP BY sales_2016.county, sales_2016.store_number
ORDER BY 4;
```

The results reveal very low demand for the highest priced items. In the all of **Linn** county, only nine bottles of this price bracket were sold in the year of 2016. The team will look closely at the carrying cost and profit margins for these items. While potentially profitable because of the high revenue contribution, the team will need to negotiate with vendors to make sure the low level of demand does mean unmerited inventory carrying cost.
