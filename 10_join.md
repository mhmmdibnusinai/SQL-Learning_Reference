![Imgur](https://i.imgur.com/ug9jO4r.png)

# IV.1. Queries which Combine Tables

**SQL: Combining Data from Multiple Tables of Iowa Liquor Product Sales**

-------------

**Introduction**

Just as we organize belongings into separate storage areas, data needed for analysis is often stored in multiple locations. SQL enables you to easily combine data from multiple resources, if you have a unique identifier to bridge between the data tables. Connecting data sources is established with the `FROM` clause, identifying the source tables and the fields which are candidates for the unique connection. We’ll begin with the `INNER JOIN`, which returns only the records that match exactly from both tables. The basic command frame for the `SELECT` statement remains the same, with some new additions. When referencing columns within a `JOIN` query, use the formal labeling for column names, meaning table name together with column name separated by a period, for clear identification. The structure to `JOIN` two tables within the `FROM` clause is accomplished as follows:


    FROM <primary table name here>
    INNER JOIN <secondary table here>
    ON primary_table.field_from_primary = secondary_table.field_from_secondary

**Scenario**

You’ve been asked to assist a business development team looking at investing in a new retail location in Iowa. Based on their current plan, alcohol sales will represent a valuable margin producing portion of their product offering. As a result, they are very interested in gaining insights on their potential competitors in areas with large sales and substantial population bases.

To begin a new SQL query, click “New query” from the queries pane on the left side of the workspace. Choose “SQL query” from the drop-down menu that is presented. A new window opens, providing you the opportunity to enter your commands. The available data elements are listed in the Dataset schema pane on the right edge, along with symbols describing their data type. Next to the “1.” in the newly opened window, begin your query.


1. Let’s begin by identifying the top revenue producing stores in Iowa. In your output, include the store name and address from the stores table, along with the store number and total sales available in the sales tables. To accomplish this, JOIN the sales and stores tables together based on the store ID number and calculate the sum of total sales for each store. The aggregation of sales implies you will GROUP BY each store. Finish by adding an ORDER BY statement that places the most successful stores at the top of your report. Your finished query will look like this:


       SELECT sales_2016.store_number, stores.name, stores.store_address,
       SUM (sales_2016.sale_dollars)AS total_sold
       FROM sales_2016 INNER JOIN stores
       ON sales_2016.store_number = stores.store
       GROUP BY sales_2016.store_number, stores.name, stores.store_address
       ORDER BY total_sold DESC
       LIMIT 10;
  
_As you review the top stores by gross sales, you will notice that their city names correlate with our prior query insights about top producing counties. Your investment team will also observe that these are big box operators and a local grocery giant. The team is interested in other perspectives the data might have to offer._


2. Slicing the data from a different point of view, we can see the amount of money spent on alcohol purchases per capita within the various Iowa counties. (Notice that the sample query implements aliasing of the table names and the USING command to identify the connection between the tables, as the connecting columns in these two tables are titled the same way.) Your query will contain the following:


       SELECT a.county, SUM(a.sale_dollars) AS total_sold, b.population,
       ROUND((SUM(a.sale_dollars)/(b.population)),2) AS per_capita_spend
       FROM sales a INNER JOIN counties b
       USING(county)
       GROUP BY a.county, b.population
       ORDER BY per_capita_spend DESC
       LIMIT 10;
  
_Based on this analysis, it is interesting to observe that Dickinson, a smaller county, spends a disproportionately large amount on alcohol per person in that county. Further analysis would be required to rule out possibility temporary conditions that could skew the results, such as seasonal festivals or college student bodies._


![](https://d2mxuefqeaa7sj.cloudfront.net/s_11A3C926B66539979AA819795B9AE12298B748340FB3979DD3408D5E6794E6C3_1515399492094_file.png)

3. To further prepare the competitive survey for the investment team, let’s query for the number of stores in the top four counties from the per capita spend query. Include the total sales summation for each county, their respective populations, and calculate the number of people for each available store for context. Your query will look like this:


       SELECT sales_2016.county,
       COUNT(DISTINCT sales_2016.store_number) AS qty_stores, 
              SUM(sales_2016.sale_dollars) AS total_sales, 
              counties.population AS county_population, 
              (counties.population/(COUNT(DISTINCT sales_2016.store_number))) AS num_people_per_store
       FROM sales_2016 INNER JOIN counties USING(county)
       WHERE sales_2016.county IN('Dickinson', 'Polk', 'Johnson', 'Cerro Gordo' )
       GROUP BY sales_2016.county, counties.population
       ORDER BY total_sales DESC;
  
_Based on this analysis, Johnson county may represent an opportunity in terms of the combined factors that would begin the conversation of demand analysis and sufficient population for sustainability. Further analysis would be merited, based on the investor’s risk profile and interests._

![](https://d2mxuefqeaa7sj.cloudfront.net/s_11A3C926B66539979AA819795B9AE12298B748340FB3979DD3408D5E6794E6C3_1515399492098_file.png)
