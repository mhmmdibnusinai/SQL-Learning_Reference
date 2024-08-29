# III.2. SQL Aggregating and Grouping: Diving Deeper

Here are two ways to take your query to the next level by `JOIN`ing additional information from reference tables and by filtering data using compound `WHERE` conditional statements.


- The management team wants a query that confirms that the population is large enough to provide a substantial customer base. Combine the population values from the Counties table with the transactions in the Sales table using a LEFT JOIN, filtering for the two top products your query results led you to in question #2. After aggregating by county, add an additional criterion of a population size of at least 150,000.


       SELECT DISTINCT sales.county, counties.population,
       COUNT(sales.item) as qty_sold, SUM(sales.total) as total_sold
       FROM sales LEFT JOIN counties USING(county)
       WHERE description IN('Black Velvet', 'Hawkeye Vodka')
       GROUP BY sales.county, counties.population
       HAVING counties.population>150000
       ORDER BY total_sold DESC;

This analysis shows that the Polk, Linn and Scott counties offer the greatest market opportunity based on volume of potential customers, already enjoying the top products. You have confirmed your earlier insight of Polk and Linn counties being a profitable area.


- Finally, you’ve been asked to size up the volume of other products that are sold in these three counties to test for supporting the breadth of the business in these areas. Your query may look like:


       SELECT DISTINCT sales.county,
       COUNT(sales.item) as qty_sold,
       SUM(sales.total) as total_sold
       FROM sales
       WHERE sales.description NOT IN('Black Velvet','Hawkeye Vodka')
       AND sales.county IN('Polk', 'Linn', 'Scott')
       GROUP BY sales.county
       ORDER BY total_sold desc;

From this, you observe that the product sales base in the top three counties is much broader than the top products. For example, in Polk county, while the top two products consist of 28k sales, there are 534k other product sales transactions. Well done, you’re making progress toward identifying the characteristics of what is performing well in Iowa’s liquor marketplace.

# [**Continue**](https://data.world/classrooms/guide-to-data-analysis-with-sql-and-datadotworld/workspace/file?filename=09_aggregate_and_group_summary.md)