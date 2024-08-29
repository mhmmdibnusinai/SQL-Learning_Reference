![Imgur](https://i.imgur.com/JKEwxTg.png)

# III.1. Aggregating and Grouping Data

**dwSQL: Querying Product and Sales Data**       
**Iowa Liquor Product Sales**         

---------------

**Introduction**         
Aggregation SQL functions bring clarity and depth to queries, which include `DISTINCT`, `COUNT`, `GROUP BY` and `HAVING`. These commands add to the filtering accomplished by the `WHERE` clause, and enable viewing data in groups, segments or other organized levels. `DISTINCT` and `COUNT` are often used in the `SELECT` statement to create and quantify aggregation. By contrast, `GROUP BY` and `HAVING` are placed after the `WHERE` clause. As you consider using these tools in your query, it is important to be consistent in the level of aggregation requested in one query. The following shows the appropriate order for the command tools in your new query:

    SELECT DISTINCT <desired column list>
    FROM <table name here>
    WHERE <filtering criteria>
    GROUP BY <data_name> HAVING <additional filter>

**Scenario**             
You’ve been asked to assist a regional product management team that is responsible for retail inventory distribution for Iowa liquor sales. They’ve asked you to apply your emerging SQL skills to create queries that build toward a profile of “what’s successful” in the territory of Iowa. Fortunately, any retailer with a class E liquor license in Iowa reports sales transactions into the state which in turn makes the data publicly available on their Iowa.gov website.

To begin a new SQL query, click “New query” from the queries pane on the left side of the workspace. Choose “SQL query” from the drop-down menu that is presented. A new window opens, providing you the opportunity to enter your commands. The available data elements are listed in the Dataset schema pane on the right edge, along with symbols describing their data type. Next to the “1.” in the newly opened window, begin your query.


1. Let’s begin by identifying the top selling items, regardless of vendor or category. Let’s measure the quantity of transactions, average transaction price and total sales. Your query will contain the following:


       SELECT DISTINCT sales.item, sales.description, COUNT(sales.item) as qty_sold,
       ROUND(AVG(sales.total),2) as avg_transaction_price, SUM(sales.total) as total_sold
       FROM sales
       GROUP BY sales.item, sales.description
       ORDER BY COUNT(sales.item) DESC
       LIMIT 10;

Based on this analysis, you observe that the top two popular products are Black Velvet and Hawkeye Vodka. As you look down the top ten list, it is interesting to see that Black Velvet Whiskies have been sold under two separate product numbers, which reveals that they are the most popular product by a large margin, when added together.


2. The product marketing team now wants to know where the top two products have the highest volume of transactions. Your query will look like this:


       SELECT DISTINCT sales.county, COUNT(sales.item) as qty_sold,
       SUM(sales.total) as total_sold
       FROM sales
       WHERE description IN('Black Velvet', 'Hawkeye Vodka')
       GROUP BY sales.county HAVING (COUNT(sales.item))>10000
       ORDER BY total_sold DESC;


This analysis shows that Polk and Linn counties have the highest transaction volume for the top two products you discovered in an earlier query.


3. As the product marketing team evaluates the channel partners, they want to know the top ten vendors with the broadest product lineup. Create a count of the products offered by each vendor and the average bottle price from the products table. Notice that the bottle price is “casted” in the example query into a decimal, which is required averaging and rounding. Your query will look like this:


       SELECT products.vendor_name, COUNT(*) AS products_offered,
       ROUND(AVG(CAST(products.bottle_price AS DECIMAL)),2) AS avg_price
       FROM products
       GROUP BY products.vendor_name
       ORDER BY products_offered DESC
       LIMIT 10;

Rank the vendors a second time by the average bottle price they offer for resale and observe any significant changes in the order of the list.

# [**Continue**](https://data.world/classrooms/guide-to-data-analysis-with-sql-and-datadotworld/workspace/file?filename=08_aggregate_and_group_2.md)